FROM jenkins/jenkins:lts

USER root

ENV DOCKERVERSION=24.0.1

RUN curl -fsSLO https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKERVERSION}.tgz \
  && tar xzvf docker-${DOCKERVERSION}.tgz --strip 1 \
                 -C /usr/local/bin docker/docker \
  && rm docker-${DOCKERVERSION}.tgz

RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl \
	&& chmod +x ./kubectl \
	&& mv ./kubectl /usr/local/bin/kubectl

RUN curl -sL https://deb.nodesource.com/setup_18.x | bash - && apt-get install -y nodejs

RUN curl https://github.com/snyk/cli/releases/download/v1.1167.0/snyk-alpine -o snyk &&\
  chmod +x ./snyk &&\
  mv ./snyk /usr/local/bin/