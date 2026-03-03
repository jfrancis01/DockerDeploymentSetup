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

TO DO: 
1. Add Nginx server service to host and serve the static Angular pages of the application
2. Write A Jenkins job to build and deployb the Angular application 
