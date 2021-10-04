# TeamCity

In this tutorial we'll setup an instance of TeamCity, running on Docker, and configure a CI pipeline on the code samples available in this repository.

## Introduction

Read more from [everyday-cheatsheets](https://github.com/devpro/everyday-cheatsheets/blob/main/docs/jetbrains/teamcity.md)

## Setup

### Run TeamCity server in a container

```bash
# creates local folders to store TeamCity run files
md data logs

# starts the container
docker run -it --name teamcity-server -v $PWD/data:/data/teamcity_server/datadir -v $PWD/logs:/opt/teamcity/logs -p 8111:8111 jetbrains/teamcity-server

# if stopped, starts again the container
docker start teamcity-server
```

### Run TeamCity agent in a container

```bash
md agent
md agent/conf

docker run -it --name teamcity-agent -e SERVER_URL="http://localhost:8111" -v $PWD/agent/conf:/data/teamcity_agent/conf jetbrains/teamcity-agent
```

→ [hub.docker.com](https://hub.docker.com/r/jetbrains/teamcity-agent/)

### First TeamCity configuration

* Open [localhost:8111](http://localhost:8111)
* Log-in with empty username and the token shown in the container log file
* Go to the "Administration" section and click on "Users" to create a new user account (make sure to give the super administrative privilege)
* Go to the "Projects" section and create a new project (use `https://github.com/devpro/ci-tools-demo` as repository URL)
* Use "Auto-detected Build Steps" to have TeamCity review what is needed (you can select everything except the NuGet and msbuild step)
* Review steps and step names and start a new build
