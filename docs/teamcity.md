# TeamCity

In this tutorial we'll setup an instance of TeamCity, running on Docker, and configure a CI pipeline on the code samples available in this repository.

## Introduction

Read more from [everyday-cheatsheets](https://github.com/devpro/everyday-cheatsheets/blob/main/docs/jetbrains/teamcity.md)

## Setup

### Create local folders

```bash
# creates local folders to store TeamCity run files
md data logs agent
md agent/conf
```

### Run TeamCity server in a container

```bash
# starts the container
docker run -it --name teamcity-server -v $PWD/data:/data/teamcity_server/datadir -v $PWD/logs:/opt/teamcity/logs -p 8111:8111 jetbrains/teamcity-server

# if stopped, starts again the container
docker start teamcity-server
```

### Run TeamCity agent in a container

#### Use the Docker image

```bash
docker run -it --name teamcity-agent -e SERVER_URL="http://teamcity-server:8111" -v $PWD/agent/conf:/data/teamcity_agent/conf --link teamcity-server jetbrains/teamcity-agent
```

→ [hub.docker.com](https://hub.docker.com/r/jetbrains/teamcity-agent/)

#### Use a custom image

The default image may not contain all the needed tools for the pipeline to run (for example .NET 5 in September 2021).

TODO

### Clean-up

```bash
docker rm teamcity-server teamcity-agent
```

## Configuration

### First TeamCity configuration

* Open [localhost:8111](http://localhost:8111)
* Log-in with empty username and the token shown in the container log file
* Go to the "Administration" section and click on "Users" to create a new user account (make sure to give the super administrative privilege)
* Authorize the agent [localhost:8111/agents](http://localhost:8111/agents.html?tab=unauthorizedAgents)
* Go to the "Projects" section and create a new project (use `https://github.com/devpro/ci-pipeline-samples` as repository URL)
* Use "Auto-detected Build Steps" to have TeamCity review what is needed (you can select everything except the NuGet and msbuild step)
* Review steps and step names and start a new build
