
1. `FROM`
Purpose: Specifies the base image to use for the Docker image.

- **Usage**: 
  ```Dockerfile
  FROM ubuntu:20.04
  ```

2. `RUN`
Purpose: Executes commands in a new layer on top of the current image and commits the results. Often used for installing software packages.

- **Usage**: 
  ```Dockerfile
  RUN apt-get update && apt-get install -y nginx
  ```

3. `COPY`
Purpose: Copies files and directories from the host filesystem to the image filesystem.

- **Usage**: 
  ```Dockerfile
  COPY src/ /app/
  ```

4. `ADD`
Purpose: Similar to `COPY` but also supports extracting compressed files (e.g., .tar).

- **Usage**: 
  ```Dockerfile
  ADD my_archive.tar.gz /app/
  ```

5. `CMD`
Purpose: Provides the default command to run when a container starts. This can be overridden by command line arguments when `docker run` is executed.

- **Usage**: 
  ```Dockerfile
  CMD ["nginx", "-g", "daemon off;"]
  ```

6. `ENTRYPOINT`
Purpose: Configures a container that will run as an executable. Unlike `CMD`, `ENTRYPOINT` is not overridden by command line arguments.

- **Usage**: 
  ```Dockerfile
  ENTRYPOINT ["python", "app.py"]
  ```

7. `ENV`
Purpose: Sets environment variables in the container.

- **Usage**: 
  ```Dockerfile
  ENV APP_ENV=production
  ```

8. `EXPOSE`
Purpose: Informs Docker that the container listens on the specified network ports at runtime. This is a documentation feature and does not actually publish the ports.

- **Usage**: 
  ```Dockerfile
  EXPOSE 80
  ```

9. `VOLUME`
Purpose: Creates a mount point with the specified path and marks it as holding externally mounted volumes from native host or other containers.

- **Usage**: 
  ```Dockerfile
  VOLUME /var/lib/mysql
  ```

10. `WORKDIR`
Purpose: Sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, and `ADD` instructions that follow it in the Dockerfile.

- **Usage**: 
  ```Dockerfile
  WORKDIR /app
  ```

11. `USER`
Purpose: Sets the user name (or UID) and optionally the user group (or GID) to use when running the image and for any `RUN`, `CMD`, and `ENTRYPOINT` instructions that follow it in the Dockerfile.

- **Usage**: 
  ```Dockerfile
  USER appuser
  ```


Example Dockerfile:
Here is an example of a complete Dockerfile using some of these instructions:
```Dockerfile
# Use an official Python runtime as a parent image
FROM python:3.8-slim

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy the current directory contents into the container at /usr/src/app
COPY . .

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

Understanding and using these Dockerfile instructions effectively can help you create optimized, maintainable, and efficient Docker images.
