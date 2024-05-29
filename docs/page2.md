### Docker Compose
“Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.”

To run our entire application together, i.e run all containers parallelly, we need to configure the docker-compose file.

In the main directory of the project, (outside the server/client) create a file named docker-compose.yml .
Write these contents into the file.
```
version: '3.7'

services:
  server:
    build:
      context: ./server
      dockerfile: Dockerfile
    image: myapp-server
    container_name: myapp-node-server
    command: /usr/src/app/node_modules/.bin/nodemon server.js
    volumes:
      - ./server/:/usr/src/app
      - /usr/src/app/node_modules
    ports:
      - "5000:5000"
    depends_on:
      - mongo
    env_file: ./server/.env
    environment:
      - NODE_ENV=development
    networks:
      - app-network
  mongo:
    image: mongo
    volumes:
      - data-volume:/data/db
    ports:
      - "27017:27017"
    networks:
      - app-network
  client:
    build:
      context: ./client
      dockerfile: Dockerfile
    image: myapp-client
    container_name: myapp-react-client
    command: npm start
    volumes:
      - ./client/:/usr/app
      - /usr/app/node_modules
    depends_on:
      - server
    ports:
      - "3000:3000"
    networks:
      - app-network

networks:
    app-network:
        driver: bridge

volumes:
    data-volume:
    node_modules:
    web-root:
      driver: local
```
Creating the Build
To create the build for the entire application, we need to run the following command: ```docker-compose build```

Starting the Services
We can start the multi-container system using the following simple command: ```docker-compose up```

At last, we can open ```http://localhost:3000``` to see our React Frontend.

The backend server is live on ```http://localhost:5000```

And MongoDB is running on ```http://localhost:27017```


## Maintenance & Inspection
We can inspect running services using the ```docker-compose ps``` command.

The docker-compose logs will dump logs of all the running services.

## Stopping the containers
To stop all the services, we use ```docker-compose stop```.

Using ```docker-compose down --volumes``` brings everything down, removing the containers entirely, with the data volume of the services.