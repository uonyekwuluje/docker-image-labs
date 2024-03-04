# Java Docker Build Tools

## Build Image
Run the command below to build the image
```
docker build -t sonnar-scanner-image .
```

## List Image
List the images `docker image ls`
```
REPOSITORY              TAG       IMAGE ID       CREATED          SIZE
jdk18-mvn-image         latest    56b89eab145f   14 seconds ago   1.87GB
fedora                  35        b97d90f26110   4 weeks ago      153MB
```

## Test Image
Create a container and test the image. `docker run -ti jdk18-mvn-image bash`
```
# node --version
v21.6.1

# java -version
java version "21.0.2" 2024-01-16 LTS
Java(TM) SE Runtime Environment (build 21.0.2+13-LTS-58)
Java HotSpot(TM) 64-Bit Server VM (build 21.0.2+13-LTS-58, mixed mode, sharing)

# mvn --version
Apache Maven 3.9.6 (bc0240f3c744dd6b6ec2920b3cd08dcc295161ae)
Maven home: /opt/apache-maven
Java version: 21.0.2, vendor: Oracle Corporation, runtime: /opt/jdk-21
Default locale: en, platform encoding: UTF-8
OS name: "linux", version: "6.1.0-18-amd64", arch: "amd64", family: "unix"

# sonar-scanner --version
INFO: Scanner configuration file: /opt/sonar-scanner/conf/sonar-scanner.properties
INFO: Project root configuration file: NONE
INFO: SonarScanner 5.0.1.3006
INFO: Java 17.0.7 Eclipse Adoptium (64-bit)
INFO: Linux 6.1.0-18-amd64 amd64
```
