# jenkins/Dockerfile

FROM jenkins/jenkins:lts-jdk17
USER root

# Docker 공식 repo 등록 + CLI/플러그인 설치 (항상 /usr/bin/docker 제공)
RUN apt-get update \
 && apt-get install -y --no-install-recommends ca-certificates curl gnupg jq \
 && install -m 0755 -d /etc/apt/keyrings \
 && curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg \
 && chmod a+r /etc/apt/keyrings/docker.gpg \
 && . /etc/os-release \
 && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian ${VERSION_CODENAME} stable" > /etc/apt/sources.list.d/docker.list \
 && apt-get update \
 && apt-get install -y --no-install-recommends docker-ce-cli docker-buildx-plugin docker-compose-plugin \
 && rm -rf /var/lib/apt/lists/*

# 호스트 docker.sock GID와 맞추기
ARG DOCKER_GID=0
RUN if [ "$DOCKER_GID" = "0" ]; then \
      usermod -aG root jenkins; \
    else \
      groupadd -g "$DOCKER_GID" docker 2>/dev/null || true; \
      usermod -aG docker jenkins; \
    fi


USER jenkins