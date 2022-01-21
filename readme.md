# selenium-test-runner

## Project Structure:

This project consists of a docker-compose.yml to quickly spin-up a selenium grid and run tests on it. Additionally, there is a Jenkinsfile which Jenkins jobs will use to pull the latest test script image from docker hub, start the selenium grid, run the tests, and then bring everything down and archive the test results

## Docker-Compose.yml
#### Services:
- hub
- chrome
- firefox
- tests

If running the tests on your local machine (not in aws) then you can expose port 4444 via the docker-compose.yml in order to see the status of the grid via the grid console: 
`localhost:4444/grid/console`

AND/OR

To allow external nodes to be able to connect.

NOTE: there are two docker-compose.yml files located in this project. One is for selenium grid 3.141.59 - the current LTS. The other is for selenium grid 4.1.0 the new version of selenium grid that will be replacing the LTS version. Only the file titled "docker-compose.yaml" will be used by the `docker compose` commands.

##### Scaling Services
In order to execute tests in parallel, it is necessary to scale the number of browser instances to match the desired number of tests you want to run.
To scale your selenium grid services use the --scale flag with your docker-compose up command. 

ex:
`docker-compose up -d --scale chrome=4 --scale firefox=4`

This command will bring up 4 instances of the chrome service and 4 instances of the firefox service in background mode( -d ).

#### Connecting to an External Hub / Node
To connect an external node to our dockerized hub:
1. Make sure the hub is running `docker-compose up -d hub`
2. One or more Nodes can be started in this setup, and the server will detect the available drivers that it can use from the System PATH.

`java -jar selenium-server-<version>.jar -role node -hub http://<host-ip>:4444/grid/register`

`appium -p 4725 --nodeconfig <path/to/node-config.json>`

To connect an external hub to our dockerized nodes:
1. Start the external hub `java -jar selenium-server-standalone-<version>.jar -role hub`
2. Start the chrome and firefox services

NOTE: make sure to update the docker-compose.yml to set the service environment variables for HUB_HOST to the ip / hostname of the external hub machine

#### Running tests inside a Docker Container

##### Docker-Compose

## Jenkinsfile



https://www.selenium.dev/documentation/grid/



