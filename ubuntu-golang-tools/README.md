# Ubuntu Golang Base Image & Tools

## Build Image
Run the command below to build the image
```
docker build -t golang-tools .
```

## List Image
List the images
```
docker images
```

## Test Image
```
docker run -ti golang-tools bash
```

## Inspect Image
```
docker inspect  golang-tools
docker inspect --format "{{ index .Config.Labels \"build_version\" }}" golang-tools
```
