# Build a docker image for tierpsy

### Build:
**From tierpsy-tracker folder**:
``` bash
docker build -t tierpsy-tracker . -f docker/Dockerfile
```

### Run:
``` bash
./run_tierpsy_docker.sh
```
(look inside the script for details)

### Tag:
``` bash
docker tag tierpsy-tracker tierpsy/tierpsy-tracker
docker tag tierpsy-tracker tierpsy/tierpsy-tracker:1.5.2
```
The one without tag defaults to `:latest`.

### Publish
``` bash
docker push tierpsy/tierpsy-tracker
docker push tierpsy/tierpsy-tracker:1.5.2
```