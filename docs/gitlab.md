# GitLab

## Run locally the pipeline

```bash
# creates local folder
mkdir -p .gitlab/runner/local

# runs build job
docker run --rm --name gitlab-runner --workdir $PWD \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v $PWD/.gitlab/runner/local/config:/etc/gitlab-runner \
  -v $PWD:$PWD \
  gitlab/gitlab-runner exec docker ci
```
