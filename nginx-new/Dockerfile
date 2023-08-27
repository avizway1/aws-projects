# Use a multi-architecture base image
FROM nginx:alpine

# Copy your HTML file into the Nginx server's root directory
COPY index.html /usr/share/nginx/html/

# Expose port 80 (the default Nginx port)
EXPOSE 80

# Start Nginx when the container starts
CMD ["nginx", "-g", "daemon off;"]

