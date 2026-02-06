# -------- Build Stage --------
# Use a lightweight Node.js image to build the Angular app
FROM node:20-alpine AS build

# Set the working directory inside the container to /app
WORKDIR /app

# Copy package.json and package-lock.json (if present) for dependency installation
COPY package*.json ./

# Install all dependencies defined in package.json
RUN npm install

# Copy the rest of the application source code into the container
COPY . .

# Build the Angular app for production (output goes to /app/dist)
RUN npm run build --prod

# -------- Production Stage (Nginx) --------
# Use a lightweight Nginx image to serve the built Angular app
FROM nginx:alpine

# Copy the built Angular files from the build stage to the Nginx web root
COPY --from=build /app/dist /usr/share/nginx/html

# Expose port 80 (default HTTP port)
EXPOSE 80

# Start Nginx in the foreground (so the container keeps running)
CMD ["nginx", "-g", "daemon off;"]