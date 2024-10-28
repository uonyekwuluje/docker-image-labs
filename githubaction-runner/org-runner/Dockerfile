FROM ubuntu:22.04
MAINTAINER Uchechukwu Onyekwuluje <uionyekwuluje@gmail.com>

ARG RUNNER_VERSION="2.320.0"

RUN apt-get update -y && apt-get upgrade -y && useradd -m builder
RUN apt-get install -y pass software-properties-common curl wget jq build-essential unzip libssl-dev libffi-dev python3 python3-pip apt-utils \
    && pip install yq docker ansible --user

# Install Latest Git
RUN add-apt-repository ppa:git-core/ppa -y \
    && apt update && apt install -y git

RUN mkdir /opt/actions-runner && cd /opt/actions-runner \
    && curl -O -L https://github.com/actions/runner/releases/download/v${RUNNER_VERSION}/actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz \
    && tar xzf ./actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz
RUN chown -R builder:builder /opt/actions-runner && /opt/actions-runner/bin/installdependencies.sh

COPY start.sh start.sh

RUN chmod +x start.sh

USER builder

ENTRYPOINT ["./start.sh"]
