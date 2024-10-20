## 1. Choice of Base Image
**Frontend Container:**

The base image selected for the build stage is node:16-alpine, which is a lightweight version of the Node.js environment. I used alpine because it is lightweight hence reducing image size and  it's secure as it does not come with all the libraries that may have security issues.
For the final stage, I am using the nginx:alpine image. I tried using port 3000 but it was not working and decided to use nginx since it also serve the frontend assets well. Using the Alpine version for nginx ensures that the production image remains lightweight.

**Backend Container:**

The base image node:14 is chosen for the backend. A multi-stage build is used, where the application is initially built using this Node.js image, and then transferred to the final stage, which uses alpine:3.16.7. This ensures that the final image is smaller, as unnecessary dependencies are discarded.

**MongoDB Container:**
I am using the official mongo image for the database service. It's well maintained and secure since it's from mongo themselves.

For both the frontend and backend I am building multi-stage images to reduce the size of the final image.

## 2. Dockerfile directives used in the creation and running of each container.
I have two dockerfiles, one for frontend and the other for backend.

**Frontend Dockerfile:**

The build stage sets up the Node.js environment, installs dependencies, and builds the production files using npm run build.

In the final stage, the nginx container is configured to serve the built files by copying them into Nginx’s default static file directory (/usr/share/nginx/html).
The directive EXPOSE 80 is used to expose port 80, and the command nginx -g 'daemon off;' ensures that Nginx runs in the foreground.

**Backend Dockerfile:**

The build stage uses npm install to install dependencies and prepares the application code.
The final stage copies the built app into a clean alpine container and exposes port 5000, where the backend API will run.
The command npm start is used to start the Node.js server.

## 3. Docker Compose Networking (Application port allocation and a bridge network implementation)
The Docker Compose file sets up a bridge network called app-net.
This network allows the frontend, backend, and MongoDB containers to communicate with each other securely.

**Port Allocation:**
The frontend service runs on port 80, which is exposed to the host.
The backend service runs on port 5000, which is also exposed to the host.
The MongoDB database is exposed on port 27017.
Each service is assigned to the app-net network, and the IP address range is explicitly defined via the ipam configuration to ensure predictable IP allocation and easy debugging.

## 4. Docker-compose volume definition and usage
The MongoDB service uses a Docker volume to persist data between container restarts.

The volume is named app-mongo-data and is mapped to /data/db inside the MongoDB container, which is where MongoDB stores its database files.
This ensures that data is not lost if the container is removed or recreated, making it a critical part of the service's durability.

## 5. Git workflow used to achieve the task

**Forking and Cloning:**

Fork the Shared Repository: 
I started by forking the repository to create a personal copy in my GitHub account. This allowed me to make changes without affecting the original project directly.
Clone to Local Machine: 
I cloned the forked repository to my local machine using the command git clone git@github.com:ChepkemoiPeris/yolo.git. This step gave me a local copy of the repository where I could begin working on the Docker-related tasks.

**Commit Breakdown:**

Commit 1: update backend Dockerfile and remove unnecessary file:

In this commit, I updated the backend Dockerfile to optimize the image. I removed unnecessary files or instructions that were no longer relevant to the backend build. 

Commit 2: update files to add/match created image path:

After building the Docker images, I ensured that the paths in the Dockerfiles and Docker Compose YAML file were updated to match the generated image paths. This was necessary to correctly reference the Docker images in the Docker Compose setup.

Commit 3: update frontend dockerfile and yaml file to match generated image:

I made similar changes to the frontend Dockerfile and Docker Compose YAML file to match the frontend image paths, ensuring consistency between the local and DockerHub versions.

Commit 4: remove version attribute on yaml file:

I noticed that the version attribute in the docker-compose.yaml was unnecessary (as it is deprecated in newer versions of Docker Compose). I removed it to avoid compatibility issues and to follow best practices with Docker Compose syntax.

Commit 5: update frontend and backend docker files and update image versions on docker-compose.yaml:

This commit involved updating both the frontend and backend Dockerfiles and ensuring the correct image versions were referenced in the Docker Compose YAML file. By keeping image versions consistent, I ensured smooth integration and version control.

Commit 6: updating Dockerfile to use nginx and port 80 instead of 3000:

In this commit, I updated the frontend Dockerfile to use Nginx and expose port 80 instead of port 3000 (which is typically used for development and had many issues I couldn't run on container). 

Commit 7: update the explanation.md and readme also screenshot of hosted docker images:

After finalizing the Docker setup, I documented the process. I updated the explanation.md file to explain the rationale behind my changes and included a README update with the necessary instructions for running the containers. I also included a screenshot of the hosted Docker images on DockerHub to demonstrate that the images were successfully pushed and tagged.

