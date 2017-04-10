FROM jenkins:2.32.2-alpine

USER root
RUN apk update && apk add curl python && curl -o docker-1.12.1.tgz https://get.docker.com/builds/Linux/x86_64/docker-1.12.1.tgz && tar zxvf docker-1.12.1.tgz && mv docker/* /usr/bin/ && rm -rf docker-*
RUN curl -o wget https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-150.0.0-linux-x86_64.tar.gz \
	&& tar -xvzf google-cloud-sdk-150.0.0-linux-x86_64.tar.gz \
	&& rm google-cloud-sdk-150.0.0-linux-x86_64.tar.gz \
	&& ./google-cloud-sdk/install.sh \
	&& ./google-cloud-sdk/bin/gcloud init
# Add support for proxies.
# Values should be passed as build args
# http://docs.docker.com/engine/reference/builder/#arg
ENV http_proxy ${http_proxy:-}
ENV https_proxy ${https_proxy:-}
ENV no_proxy ${no_proxy:-}
ARG JAVA_OPTS
ENV JAVA_OPTS "-Djenkins.install.runSetupWizard=false ${JAVA_OPTS:-}"

# Define build argument and env variable master_image_version
# Use to pass info about Jenkins version so it can be represented
# for the users.
ARG master_image_version="local build"
ENV master_image_version $master_image_version

# Install plugins
COPY plugins.txt /usr/share/jenkins/plugins.txt
RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt

# Copy configuration scripts that will be executed by groovy
COPY *.groovy /usr/share/jenkins/ref/init.groovy.d/
COPY *.properties $JENKINS_HOME/
