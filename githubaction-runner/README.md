# Docker Github Runner

## Build Image
Run the command below to build the image
```
docker build -t github-runner-image .
```

## List Image
List the images
```
docker images

REPOSITORY            TAG       IMAGE ID       CREATED              SIZE
github-runner-image   latest    0802afd2374b   About a minute ago   1.06GB
```

## Test Image
Login to Image
```
docker run -ti github-runner-image bash
```

## Release Links
* [Gitlab Runner Repo](https://github.com/actions/runner/releases)
