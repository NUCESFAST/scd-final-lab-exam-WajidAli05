# Use an official node image as a parent image
FROM node:14

# Set the working directory in the container
WORKDIR /app

# Copy the package.json and package-lock.json files to the working directory
COPY package*.json ./

# Install the project dependencies
RUN npm install

# Copy the rest of the application code to the working directory
COPY . .

# Expose the port the application will run on
EXPOSE 9812

# Command to run the application
CMD ["npm", "start"]
