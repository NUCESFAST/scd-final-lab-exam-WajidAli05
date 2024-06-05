# Use an official node image as a parent image
FROM node:14

# Set the working directory in the container
WORKDIR /app

# Copy the package.json and package-lock.json files to the working directory
COPY package*.json ./

# Install the project dependencies
RUN npm install

# Update Browserslist database
RUN npx browserslist@latest --update-db

# Copy the rest of the application code to the working directory
COPY . .

# Build the React application
RUN npm run build

# Install 'serve' to serve the build directory
RUN npm install -g serve

# Expose the port the application will run on
EXPOSE 9811

# Command to run the application
CMD ["serve", "-s", "build"]
