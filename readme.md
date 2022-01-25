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

The Selenium-Grid default node config only supports Firefox, Chrome, and IE. if you want to test against additional browsers like Edge, you have to create a custom node config file and point to it (and the webdriver) when running the java command to register the node.

Ex for ms edge:
```
   java -Dwebdriver.edge.driver="/path/to/msedgedriver.exe" -jar selenium-server-standalone-3.141.59.jar -role node -nodeConfig "/path/to/custom-node-config.json"
```

see: [example-custom-node-config.json](./example-custom-node-config.json)

#### Running tests inside a Docker Container

Use docker-compose up to quickly spin-up a selenium grid and run tests on it, archive the results and docker compose down bring everything down.

First the selenium grid will start. When the test scripts containers are started they will start the healthcheck.sh. The healthcheck.sh will wait until the hub is up and ready then run the tests:

```
java -cp java-selenium.jar:java-selenium-tests.jar:libs/* \
    -DHUB_HOST=$HUB_HOST \
    -DBROWSER=$BROWSER \
    org.testng.TestNG $FEATURE
```

This command places our page object .jar, test .jar, and external dependency jars on the class path and sets the environment variables for the hub host, the browser, and test feature -- all passed in from the docker-compose.yml. The $FEATURE points to the test-suite.xml file which has all to the test case parameters.

##### Docker-Compose

docker compose is used for our container orchestration. 

## Jenkinsfile

The Jenkins file will be used be a Jenkins job execution agent. It will execute the scripts listed in the order of the stages.

https://www.selenium.dev/documentation/grid/



