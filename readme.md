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

NOTE: there are two docker-compose.yml files located in this project. One is for selenium grid 3.141.59 - the current LTS. The other is for selenium grid 4.1.0 the new version of selenium grid that will be replacing the LTS version. Only the file titled "docker-compose.yaml" will be used by the `docker compose` commands.

##### Scaling Services
In order to execute tests in parallel, it is necessary to scale the number of browser instances to match the desired number of tests you want to run.
To scale your selenium grid services use --scale flag with your docker-compose up command. 

ex:
`docker-compose up -d --scale chrome=4 --scale firefox=4`

This command will bring up 4 instances of the chrome service and 4 instances of the firefox service in background mode( -d ).

#### Connecting to an External Hub / Node
#### Running tests inside a Docker Container





