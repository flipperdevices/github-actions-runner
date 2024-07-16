FROM summerwind/actions-runner:v2.317.0-ubuntu-22.04

USER root

RUN DEBIAN_FRONTEND=noninteractive apt update && apt -y install wget apt-transport-https ca-certificates curl gnupg

RUN mkdir -p /opt/archives

# OpenJDK17 temurin
RUN wget https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.11%2B9/OpenJDK17U-jdk_x64_linux_hotspot_17.0.11_9.tar.gz -O /opt/archives/openjdk.tar.gz # https://github.com/adoptium/temurin17-binaries/releases/tag/jdk-17.0.11%2B9
RUN tar -xvf /opt/archives/openjdk.tar.gz -C /opt/
ENV JAVA_HOME=/opt/jdk-17.0.11+9
ENV PATH=$JAVA_HOME/bin:$PATH

# Android Studio CommandLine Tools
ENV ANDROID_SDK_ROOT=/opt/android_sdk
ENV ANDROID_HOME=$ANDROID_SDK_ROOT
RUN mkdir -p /opt/sdk
RUN mkdir -p $ANDROID_SDK_ROOT/cmdline-tools/latest
RUN wget https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip -O /opt/archives/android_sdk.zip
RUN unzip /opt/archives/android_sdk.zip -d /opt/sdk
RUN mv /opt/sdk/cmdline-tools/* $ANDROID_SDK_ROOT/cmdline-tools/latest
RUN rm -r /opt/sdk
RUN yes | $ANDROID_SDK_ROOT/cmdline-tools/latest/bin/sdkmanager --licenses
RUN $ANDROID_SDK_ROOT/cmdline-tools/latest/bin/sdkmanager tools platform-tools
ENV PATH=$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools:$PATH

RUN mkdir -p "/etc/docker"
RUN echo "{\"mtu\": 1454}" | tee -a /etc/docker/daemon.json > /dev/null
