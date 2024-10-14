# Step 1: Build stage (using Node 16)
FROM node:16-alpine AS build

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy the package.json and package-lock.json files to the container
COPY package*.json ./

# Install production dependencies only
RUN npm install --only=production

# Copy the rest of the application code to the container
COPY . .

# Run the build command to create optimized production files
RUN npm run build
# Step 2: Final stage for production (using Alpine for a lightweight runtime)
FROM alpine:3.16

# Install Node.js and npm in the final stage
RUN apk add --no-cache nodejs npm

WORKDIR /app

# Copy built files and node_modules from the build stage
COPY --from=build /usr/src/app /app

# Expose the port the app runs on
EXPOSE 3000

# Define the command to run your app
CMD ["npm", "start"]
