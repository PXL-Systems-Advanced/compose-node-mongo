# Docker Compose exercise 2

This is a Docker compose exercise made for the PXL Docker Compose course.

## Project structure

## backend

A [node.js](https://nodejs.org/en/) [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) api based on [mongoose](https://mongoosejs.com/), which requires a [mongodb](https://www.mongodb.com/) instance.

### backend environment variables

```
MONGO_URL=mongodb://db:27017/mongo-test
```

The api server will search for a mongodb server with the host address 'db' and access it on port 27017.

The name of the api database is mongo-test and should not be changed.

```
APP_PORT=80
```

The REST api will be served on port 80.

## frontend

A react-based UI will look for the _backend_ api on http://localhost:8080. The UI itself will be served to the user on port 80 by [ngnix](https://www.nginx.com/).

## Exercise

Create a Docker compose file and make sure that:

- the application is running on docker compose and the UI is accessible through http://localhost:8880.
- the startup sequence is as follows:
  1. mongodb server
  2. backend server
  3. frontend server
- the mongodb data is stored on a docker volume named 'data'.
- environment variables are read from a file.
- two separate networks exists so the frontend server cannot reach the database server directly.

The backend service starts only when the mongodb database **service** is up and running.

Tip: [docker compose healthcheck](https://github.com/compose-spec/compose-spec/blob/master/spec.md#healthcheck)

Show real-time container statistics (CPU, memory, network, filesystem) on http://localhost:9999

---

### Dockerfile Creation Task: React Frontend with Nginx

Your task is to create a **Dockerfile** to containerize a React-based frontend application. The application has the following requirements:

1. **Development and Build Phase**:
   - Use a lightweight Node.js image (`node:12.2.0-alpine`) as the base image for building the application.
   - Set the working directory to `/app`.
   - Add the local `package.json` to the container.
   - Use the **npm** package manager to:
     - Install dependencies silently with `npm install --silent`.
     - Globally install `react-scripts` version `3.0.1` with `npm install react-scripts@3.0.1 -g --silent`.
   - Copy all local project files to the container.
   - Use the `npm run build` command to generate a production-ready build.

2. **Production Phase**:
   - Use a lightweight Nginx image (`nginx:1.16.0-alpine`) as the base image to serve the React build.
   - Copy the build output from the development stage (`/app/build`) to the Nginx HTML directory (`/usr/share/nginx/html`).
   - Expose port `80` for serving the UI.
   - Use Nginx’s default executable with the command `nginx -g "daemon off;"` to run the server.

3. **Application Behavior**:
   - The frontend UI should be accessible on port `80`.
   - The React UI should make API requests to the backend located at `http://localhost:8080`.

Your goal is to write a **multi-stage Dockerfile** that accomplishes the above. Make sure your Dockerfile is efficient and only includes the necessary files in the final production image.

### Executables to Use:
- `npm`: For dependency installation and building the React application.
- `nginx`: For serving the built frontend application.

---

### Dockerfile Creation Task: Node.js Backend Application

Your task is to create a **Dockerfile** to containerize a Node.js-based backend application. The application has the following requirements:

1. **Base Image**:
   - Use the `node:8` image as the base for the container.

2. **Application Setup**:
   - Set the working directory inside the container to `/usr/src/app`.
   - Copy the `package.json` and `package-lock.json` files (if present) to the container's working directory.
   - Use the **npm** package manager to install the application dependencies with `npm install`.

3. **Source Code**:
   - Copy the entire application source code from the local machine to the container's working directory.

4. **Port Exposure**:
   - Expose port `8080` for the application to be accessible.

5. **Start Command**:
   - Use the command `npm start` to launch the application inside the container.

