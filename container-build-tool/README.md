# Ubuntu Container Base Image & Tools

## Build Image
Run the command below to build the image
```
docker build -t cntr-build-tool .
```

## List Image
List the images
```
docker images
```

## Test Image
```
docker run -ti cntr-build-tool bash
```

## Inspect Image
```
docker inspect cntr-build-tool
docker inspect --format "{{ index .Config.Labels \"build_version\" }}" cntr-build-tool
```

## References
* [Kaniko](https://github.com/GoogleContainerTools/kaniko/releases)
