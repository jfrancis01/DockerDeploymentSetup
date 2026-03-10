This is the docker compose file that setups the following services:
a. A Jenkins Containere to build and deploy a spring boot WAR file
b. A Tomcat server that hosts the WAR file
c. A MYSQL database that us used by the srpingboot application WAR
d. An instance of KeyCloak 26.0.2 that is used by the front end Agnular application for authentication
e. A POSTGRES 15.0 db that is used by KeyCloak to store user inforamtion

DOCKER COMPOSE EXECUTION: 
1. Navigate to the folder of the docker compose file
2. Execute "docker compose up -d" without the quotes
3. Observe the services running in Docker Desktop

All the services run on the same network: dev-ops 

The Jenkins server will build and deploy the angular app to the nginx service that runs in a seprate container. Containers runb in silos and have limited communication between each other. The Angular build process creates a dist/app-name/browser folder that needs to be copied to the /user/share/nginx/html folder. 

### How is this deployment from a jenkins container to an nginx container achieved? 
This is achieved by creating a bind mount that is sahred by the two containers. Appropriate permissions need to be given to this local folder so that the containers have read write access. 

### How to configure angular app running on nginx container to communicate with keycloak on another container

### How are URLS changed for the back end spring boot application for development and production?
This is done via profiles. Create multiple "application.properties files". Ex: application-dev.properties and application-prod.properties

### How can you test the sprinboot app has been deployed correctly?
http://localhost:9000/occassionsreminder/

### How to configure angular app running on nginx container to communicate with keycloak on another container?
KC_PROXY=edge: Configures Keycloak to work behind a reverse proxy.


### How are URLS changed for the Angular application for development and production?
By creating an environments folder in the /app directory and creating an environment.ts and environment.development.ts files

events {}
http {
    server {
        listen 80;

        location /auth/ {
            # Proxy requests to the Keycloak container's internal address
            proxy_pass http://keycloak-container:8080/auth/;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Forwarded-Host $host;
            # Keycloak requires these headers
        }

        location / {
            # Serve the Angular application static files
            root /usr/share/nginx/html;
            try_files $uri /index.html;
        }
    }
}

