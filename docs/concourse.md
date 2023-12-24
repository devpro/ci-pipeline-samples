# Concourse samples

Comprehensive samples to quickly get up to speed with [Concourse](https://concourse-ci.org/)

🌐 [IT Guide](https://github.com/devpro/information-technology-guide/blob/main/docs/communities/concourse/concourse.md)

## Requirements

* Have an account to a running Concourse instance
  * For the first time, you can use the local containers with `docker compose -f .concourse/compose.yml up -d`
  * You can also deploy it in a Kubernetes cluster with [Helm chart](https://github.com/devpro/helm-charts/tree/feature/concourse/charts/concourse)
  * Ultimately, you can run it on a [VM](https://github.com/devpro/information-technology-guide/blob/main/docs/communities/concourse/ubuntu-install.md)

* Have `fly` executable on the machine running the command lines (careful with the version that needs to match the one from Concourse instance)
  * Grab it from the [releases GitHub page](https://github.com/concourse/concourse/releases) or from the running Concourse web page

## Samples

### Login

```bash
fly --target localhost login --concourse-url http://localhost:8080/
```

### Pipelines

* Hello world

Login on localhost:

```bash
fly --target localhost set-pipeline --pipeline helloworld --config .concourse/tasks/basic/01_helloworld.yml

# enables the pipeline and run it (can also be done from http://localhost:8080/teams/main/pipelines/helloworld, click on play symbol then on + symbol)
fly -t localhost unpause-pipeline -p helloworld
fly -t localhost trigger-job -j helloworld/job
```

* .NET

```bash
fly --target localhost set-pipeline --pipeline aspnetcore --config .concourse/tasks/dotnet/01_aspnetcore.yml

fly -t localhost unpause-pipeline -p aspnetcore

fly -t localhost trigger-job -j aspnetcore/build-webapp

fly --target localhost set-pipeline --pipeline dotnetglobaltool --config pipelines/dotnetcore/02_globaltool.yml --var mdbatlas-publickey=xxxx --var mdbatlas-publickey=yyyy -var almops-token=zzz --var almops-org=xxxx -var almops-user=yyyy -var almops-token=zzz

fly -t localhost unpause-pipeline -p dotnetglobaltool

fly -t localhost trigger-job -j dotnetglobaltool/mongodb-atlas -w
fly -t localhost trigger-job -j dotnetglobaltool/azure-devops -w
```

### Tasks

* Hello world

```bash
fly --target localhost login --concourse-url http://localhost:8080/

fly -t localhost execute --config .concourse/tasks/basic/helloworld.yml
```
