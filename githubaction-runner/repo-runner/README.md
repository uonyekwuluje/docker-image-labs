# Create Repo Runner

Create Docker Image
```
docker build --tag runner-image .
```

Register and start runner. 
```
docker run --detach \
--env ACCESS_TOKEN="<PAT>" \
--env OWNER="uonyekwuluje" \
--env BUILD_LABELS="build,base" \
--env REPOSITORY="software-development" \
--name software-build-runner \
runner-image
```
