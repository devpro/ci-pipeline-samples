# TeamCity

```bash
md data logs
docker run -it --name teamcity-server-instance -v ./data:/data/teamcity_server/datadir -v ./logs:/opt/teamcity/logs -p 8111:8111 jetbrains/teamcity-server
```
