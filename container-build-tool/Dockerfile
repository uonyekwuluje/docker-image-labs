FROM gcr.io/kaniko-project/executor:v1.23.2 as base
FROM ubuntu:24.04
MAINTAINER Uchechukwu Onyekwuluje <uionyekwuluje@gmail.com>

ARG BUILD_VERSION="v1.1.0"
ENV GOLANG_VERSION="1.23.2"
ENV JAVA_VERSION=21
ENV MAVEN_VERSION=3.9.6
ENV SONAR_SCANNER_VERSION=6.2.1.4610
ENV SONAR_SCANNER_HOME=/opt/sonar-scanner
ENV MAVEN_HOME=/opt/apache-maven
ENV NODEJS_VERSION=21.6.1
ENV NODEJS_HOME=/opt/nodejs
ENV TERRAFORM_VERSION="1.9.8"
ENV TERRAFORM_LOCATION="/usr/bin/terraform"
ENV PACKER_VERSION="1.11.2"
ENV PACKER_LOCATION="/usr/bin/packer"
ENV PHP_VERSION=8.3


RUN apt update -y && apt upgrade -y 
RUN apt install -y wget jq curl dnsutils iputils-ping iproute2 openssl dumb-init percona-toolkit apt-utils bind9-doc \
    bind9utils whois cracklib-runtime redis-tools net-tools dnsutils build-essential ca-certificates lsb-release unzip \
    software-properties-common apt-transport-https gnupg-agent gnupg2

# Copy the binary only from stock Kaniko image
COPY --from=base /kaniko/executor /usr/local/bin/kaniko

# Install AWS CLI
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
   unzip awscliv2.zip && \
   ./aws/install

# Install PHP
RUN add-apt-repository -y ppa:ondrej/php && \
    apt update && \
    apt install -y php$PHP_VERSION php$PHP_VERSION-gd php$PHP_VERSION-sqlite3 php$PHP_VERSION-curl \
                   php$PHP_VERSION-xml php$PHP_VERSION-mbstring php$PHP_VERSION-zip phpunit && \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
    php -r "unlink('composer-setup.php');"

# Install Terraform
RUN cd /tmp && \
    wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip && \
    unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip && \
    mv terraform ${TERRAFORM_LOCATION} && \
    rm LICENSE.txt && \
    rm /tmp/terraform_${TERRAFORM_VERSION}_linux_amd64.zip

# Install Packer
RUN cd /tmp && \
    wget https://releases.hashicorp.com/packer/${PACKER_VERSION}/packer_${PACKER_VERSION}_linux_amd64.zip && \
    unzip packer_${PACKER_VERSION}_linux_amd64.zip && \
    mv packer ${PACKER_LOCATION} && \
    rm LICENSE.txt && \    
    rm /tmp/packer_${PACKER_VERSION}_linux_amd64.zip

# Install Golang
RUN cd /tmp && \
    wget https://dl.google.com/go/go${GOLANG_VERSION}.linux-amd64.tar.gz && \
    tar -C /usr/local/ -xvf go${GOLANG_VERSION}.linux-amd64.tar.gz 

# Install Java
RUN mkdir /opt/jdk-${JAVA_VERSION} && \
    wget https://download.oracle.com/java/${JAVA_VERSION}/latest/jdk-${JAVA_VERSION}_linux-x64_bin.tar.gz && \
    tar -zxvf jdk-${JAVA_VERSION}_linux-x64_bin.tar.gz -C /opt/jdk-${JAVA_VERSION} --strip-components=1 && \
    rm jdk-${JAVA_VERSION}_linux-x64_bin.tar.gz

# Install Maven
RUN mkdir /opt/apache-maven && \
    wget https://dlcdn.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz && \
    tar -zxvf apache-maven-${MAVEN_VERSION}-bin.tar.gz -C /opt/apache-maven --strip-components=1 && \
    rm apache-maven-${MAVEN_VERSION}-bin.tar.gz

# Install Sonar Scanner
RUN rm -Rf ${SONAR_SCANNER_HOME} && \
    wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux-x64.zip && \
    unzip sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux-x64.zip && \
    mv sonar-scanner-${SONAR_SCANNER_VERSION}-linux-x64 ${SONAR_SCANNER_HOME}

# Install NodeJS
RUN rm -Rf ${NODEJS_HOME} && \
    mkdir ${NODEJS_HOME} && \
    wget https://nodejs.org/dist/v${NODEJS_VERSION}/node-v${NODEJS_VERSION}-linux-x64.tar.gz && \
    tar xzf node-v${NODEJS_VERSION}-linux-x64.tar.gz -C ${NODEJS_HOME} --strip-components=1 && \
    rm node-v${NODEJS_VERSION}-linux-x64.tar.gz

# Install Docker
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \
    add-apt-repository -y \
       "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
       $(lsb_release -cs) \
       stable" && \
    apt update && \
    apt install -y docker-ce-cli 



# Export Variables
ENV JAVA_HOME=/opt/jdk-${JAVA_VERSION}
ENV PATH $PATH:$JAVA_HOME/bin:$PATH
ENV PATH $PATH:$MAVEN_HOME/bin:$PATH
ENV PATH $PATH:${SONAR_SCANNER_HOME}/bin:$PATH
ENV PATH $PATH:${NODEJS_HOME}/bin:$PATH
ENV PATH="/root/go/bin:/usr/local/go/bin:${PATH}"

LABEL build_version="$BUILD_VERSION" 
CMD [""]
