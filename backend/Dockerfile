#----------------- Stahe 1: Build the backend application -----------------#
FROM node:21 AS backend-builder

WORKDIR /app

COPY package*.json ./

# While you only see RUN npm install (or shorthand npm i) for installing packages, the second command, RUN npm run test, is explicitly running the test scripts defined in your package.json under the "test" script.
RUN npm install

RUN npm run test

COPY . .


#----------------- Stage 2: Final stage -----------------#
FROM node:21-slim

WORKDIR /app

COPY --from=backend-builder /app ./

COPY .env.docker .env.local

EXPOSE 8080

CMD ["npm", "start"]



