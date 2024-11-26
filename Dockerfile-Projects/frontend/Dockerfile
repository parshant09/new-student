# Stage 1: Build the React app
FROM node:21-alpine3.17 as build

WORKDIR /app

# Copy the source code into the image
COPY . .

# Clear NPM cache before installing dependencies to ensure a fresh install
RUN npm cache clean --force

# Install dependencies
RUN npm install

# Update Browserslist database to avoid caniuse-lite warning
RUN npx update-browserslist-db@latest

# Build the React app
RUN npm run build

# Stage 2: Use Nginx to serve the frontend content
FROM nginx:1.25.3-alpine

# Remove default Nginx HTML
RUN rm -rf /usr/share/nginx/html/*

# Copy the build folder from the first stage to the nginx server folder
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80 for Nginx
EXPOSE 80

# Command to run Nginx
CMD ["nginx", "-g", "daemon off;"]

