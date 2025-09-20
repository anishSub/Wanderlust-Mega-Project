# ------------------- Stage 1: Build Stage ------------------------------
#Give this stage a name (frontend-builder) so we can copy files from it later.
FROM node:21 AS forntend-builder 

#Sets the working directory inside the container to /app.
WORKDIR /app

#Copies package.json and package-lock.json from your host machine into /app.
# The * wildcard means “copy both files that start with package and end with .json.”
# We copy these first so Docker can cache the dependency layer (faster builds if your code changes but dependencies don’t).
COPY package*.json ./

# Installs all dependencies defined in package.json.
# They end up inside /app/node_modules inside the image.
RUN npm install

# Copies the rest of your frontend source code (React components, public assets, etc.) into /app.
COPY . .

# ------------------- Stage 2: Final Stage ------------------------------
# Start a fresh, lighter base image (Debian “slim”) with just Node.js—much smaller than the full build image just for running the app.
FROM node:21-slim

# Again set working directory to /app in this final image.
WORKDIR /app

# Copy everything (source code) from the /app directory of the first stage (frontend-builder) into /app of this new image.
#So, the command copies everything from "/app" in the "frontend-builder" stage to the current working directory which is also set to /app in the new stage. 
COPY --from=forntend-builder /app ./

#means that during the Docker image build, the file named ".env.docker" from your local project directory is copied into the Docker image and renamed as ".env.local".
# This lets the React app pick up container-specific environment variables.
COPY .env.docker .env.local

# Documents that the container will listen on port 5173 (the default dev server port for Vite/React).
# This doesn’t actually publish the port—when you run the container you’ll map it with -p 5173:5173.
EXPOSE 5173

# The default command when the container starts:
# npm run dev – runs the “dev” script from your package.json (likely starts Vite or React dev server).
# -- --host – the extra -- passes --host to the dev server so it listens on all network interfaces (needed so you can access it from your browser outside the container).

CMD ["npm", "run", "dev", "--", "--host"]
