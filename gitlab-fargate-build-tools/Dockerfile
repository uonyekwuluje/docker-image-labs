FROM gcr.io/kaniko-project/executor:v1.23.2 as base
FROM ubuntu:24.04

ENV SONAR_SCANNER_VERSION=6.2.1.4610
ENV SONAR_SCANNER_HOME=/opt/sonar-scanner
ENV JAVA_VERSION=23

# --------------------------------------------------------------------------
# Install base packages
# --------------------------------------------------------------------------
RUN apt update && \
    apt install -y software-properties-common jq unzip apt-transport-https curl wget gnupg-agent gnupg2


# --------------------------------------------------------------------------
# Install JAVA
# --------------------------------------------------------------------------
RUN mkdir /opt/jdk-${JAVA_VERSION} && \
    wget https://download.oracle.com/java/${JAVA_VERSION}/latest/jdk-${JAVA_VERSION}_linux-x64_bin.tar.gz && \
    tar -zxvf jdk-${JAVA_VERSION}_linux-x64_bin.tar.gz -C /opt/jdk-${JAVA_VERSION} --strip-components=1 && \
    rm jdk-${JAVA_VERSION}_linux-x64_bin.tar.gz


# --------------------------------------------------------------------------
# Install Docker
# --------------------------------------------------------------------------
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \
    add-apt-repository -y \
       "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
       $(lsb_release -cs) \
       stable" && \
    apt update && \
    apt install -y docker-ce-cli 


# --------------------------------------------------------------------------
# Copy kaniko 
# --------------------------------------------------------------------------
# Copy the binary only from stock Kaniko image
COPY --from=base /kaniko/executor /usr/local/bin/kaniko


# ---------------------------------------------------------------------
# Install https://github.com/krallin/tini - a very small 'init' process
# that helps processing signalls sent to the container properly.
# ---------------------------------------------------------------------
ARG TINI_VERSION=v0.19.0
RUN apt update && \
    apt install -y curl && \
    curl -Lo /usr/local/bin/tini https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-amd64 && \
    chmod +x /usr/local/bin/tini

# --------------------------------------------------------------------------
# Install and configure sshd.
# https://docs.docker.com/engine/examples/running_ssh_service for reference.
# --------------------------------------------------------------------------
RUN apt install -y openssh-server && \
    # Creating /run/sshd instead of /var/run/sshd, because in the Debian
    # image /var/run is a symlink to /run. Creating /var/run/sshd directory
    # as proposed in the Docker documentation linked above just doesn't
    # work.
    mkdir -p /run/sshd

EXPOSE 22

# -------------------------------------------------------------------------
# Install GitLab CI required dependencies.
# https://gitlab.com/gitlab-org/gitlab-runner/-/releases
# -------------------------------------------------------------------------
ARG GITLAB_RUNNER_VERSION=v17.5.4
RUN curl -Lo /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/${GITLAB_RUNNER_VERSION}/binaries/gitlab-runner-linux-amd64 && \
    chmod +x /usr/local/bin/gitlab-runner && \
    # Test if the downloaded file was indeed a binary and not, for example,
    # an HTML page representing S3's internal server error message or something
    # like that.
    gitlab-runner --version

RUN apt install -y bash ca-certificates git git-lfs && \
    git lfs install --skip-repo


# -------------------------------------------------------------------------
# Install JAVA 
# -------------------------------------------------------------------------
RUN apt install -y openjdk-21-jdk openjdk-21-jre

# -------------------------------------------------------------------------
# NodeJS and NPM Install
# -------------------------------------------------------------------------
RUN apt install -y nodejs npm


# -------------------------------------------------------------------------
# PHP, Composer and Laravel Install
# -------------------------------------------------------------------------
ENV PHP_VERSION=8.3
RUN add-apt-repository -y ppa:ondrej/php && \
    apt update && \
    apt install -y php$PHP_VERSION php$PHP_VERSION-gd php$PHP_VERSION-sqlite3 php$PHP_VERSION-curl \
                   php$PHP_VERSION-xml php$PHP_VERSION-mbstring php$PHP_VERSION-zip phpunit && \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
    php -r "unlink('composer-setup.php');"


# -------------------------------------------------------------------------
# AWS CLI V2 Install
# -------------------------------------------------------------------------
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
   unzip awscliv2.zip && \
   ./aws/install

# -------------------------------------------------------------------------
# Sonarqube Scanner Install
# -------------------------------------------------------------------------
RUN rm -Rf ${SONAR_SCANNER_HOME} && \
    wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux-x64.zip && \
    unzip sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux-x64.zip && \
    mv sonar-scanner-${SONAR_SCANNER_VERSION}-linux-x64 ${SONAR_SCANNER_HOME}

# -------------------------------------------------------------------------------------
# Execute a startup script.
# https://success.docker.com/article/use-a-script-to-initialize-stateful-container-data
# for reference.
# -------------------------------------------------------------------------------------
COPY docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

# -------------------------------------------------------------------------
# Update/Setup Path
# -------------------------------------------------------------------------
ENV JAVA_HOME=/opt/jdk-${JAVA_VERSION}
ENV PATH $PATH:$JAVA_HOME/bin:$PATH
ENV PATH $PATH:${SONAR_SCANNER_HOME}/bin:$PATH

ENTRYPOINT ["tini", "--", "/usr/local/bin/docker-entrypoint.sh"]
#CMD [""]
