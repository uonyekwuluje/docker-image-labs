FROM fedora:39

MAINTAINER Uchechukwu Onyekwuluje <uionyekwuluje@gmail.com>

ENV JAVA_VERSION=21
ENV MAVEN_VERSION=3.9.6
ENV SONAR_SCANNER_VERSION=5.0.1.3006
ENV SONAR_SCANNER_HOME=/opt/sonar-scanner
ENV MAVEN_HOME=/opt/apache-maven
ENV NODEJS_VERSION=21.6.1
ENV NODEJS_HOME=/opt/nodejs

RUN dnf update -y && dnf upgrade -y
RUN dnf install -y unzip wget p7zip p7zip-plugins kernel-headers kernel-devel 
RUN dnf groupinstall -y "Development Tools"

RUN mkdir /opt/jdk-${JAVA_VERSION} && \
    wget https://download.oracle.com/java/${JAVA_VERSION}/latest/jdk-${JAVA_VERSION}_linux-x64_bin.tar.gz && \
    tar -zxvf jdk-${JAVA_VERSION}_linux-x64_bin.tar.gz -C /opt/jdk-${JAVA_VERSION} --strip-components=1 && \
    rm jdk-${JAVA_VERSION}_linux-x64_bin.tar.gz

RUN mkdir /opt/apache-maven && \
    wget https://dlcdn.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz && \
    tar -zxvf apache-maven-${MAVEN_VERSION}-bin.tar.gz -C /opt/apache-maven --strip-components=1 && \
    rm apache-maven-${MAVEN_VERSION}-bin.tar.gz

RUN rm -Rf ${SONAR_SCANNER_HOME} && \
    wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux.zip && \
    unzip sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux.zip && \
    mv sonar-scanner-${SONAR_SCANNER_VERSION}-linux ${SONAR_SCANNER_HOME}

RUN rm -Rf ${NODEJS_HOME} && \
    mkdir ${NODEJS_HOME} && \
    wget https://nodejs.org/dist/v${NODEJS_VERSION}/node-v${NODEJS_VERSION}-linux-x64.tar.gz && \
    tar xzf node-v${NODEJS_VERSION}-linux-x64.tar.gz -C ${NODEJS_HOME} --strip-components=1 && \
    rm node-v${NODEJS_VERSION}-linux-x64.tar.gz


ENV JAVA_HOME=/opt/jdk-${JAVA_VERSION}
ENV PATH $PATH:$JAVA_HOME/bin:$PATH
ENV PATH $PATH:$MAVEN_HOME/bin:$PATH
ENV PATH $PATH:${SONAR_SCANNER_HOME}/bin:$PATH
ENV PATH $PATH:${NODEJS_HOME}/bin:$PATH

CMD [""]
