# Bamboo Data Center

## Introduction

> Bamboo Data Center is a continuous delivery pipeline that offers resilience, reliability, and scalibility for teams of any size.

→ [atlassian.com/software/bamboo](https://www.atlassian.com/software/bamboo)

## Setup

### Build the custom image

```bash
# creates a new image
docker build . -t devprofr/bamboo-server -f .bamboo/server/Dockerfile --no-cache
```

### Run the custom image

```bash
docker volume create --name bambooVolume

# for Linux
docker run -v /var/run/docker.sock:/var/run/docker.sock -v bambooVolume:/var/atlassian/application-data/bamboo --name="bamboo" --init -d -p 54663:54663 -p 8085:8085 devprofr/bamboo-server

# for Windows
docker run -v //var/run/docker.sock:/var/run/docker.sock -v bambooVolume:/var/atlassian/application-data/bamboo --name="bamboo" --init -d -p 54663:54663 -p 8085:8085 devprofr/bamboo-server
```

### Configuration

Open [localhost:8085](http://localhost:8085/)

#### Set server capabilities

_Limitation 2021-02-28_: Unfortunately it is not possible to automate it through an API call

You have to manually go to this page ["Bamboo administration > Server capabilities"](http://localhost:8085/admin/agent/configureSharedLocalCapabilities.action) and set the server capabilities (if not present), it must be done only once/

Category   | Executable / Label | Path              | Bamboo key
-----------|--------------------|-------------------|--------------------------------
Executable | dotnet             | `/usr/bin/dotnet` | `system.builder.command.dotnet`
Docker     | Docker             | `/usr/bin/docker` | `system.docker.executable`

### Troubleshooting

```bash
docker exec -it bamboo sh
docker exec -u 0 -it bamboo bash
```

### Clean-up

```bash
docker stop bamboo
docker rm bamboo
```
