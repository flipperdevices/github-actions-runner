FROM summerwind/actions-runner-dind:v2.317.0-ubuntu-22.04

RUN DEBIAN_FRONTEND=noninteractive sudo apt update && sudo apt -y install wget apt-transport-https ca-certificates curl gnupg

RUN sudo mkdir -p /opt/archives

# OpenJDK17 temurin
RUN sudo wget https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.11%2B9/OpenJDK17U-jdk_x64_linux_hotspot_17.0.11_9.tar.gz -O /opt/archives/openjdk.tar.gz # https://github.com/adoptium/temurin17-binaries/releases/tag/jdk-17.0.11%2B9
RUN sudo tar -xvf /opt/archives/openjdk.tar.gz -C /opt/
ENV JAVA_HOME=/opt/jdk-17.0.11+9
ENV PATH=$JAVA_HOME/bin:$PATH

# Android Studio CommandLine Tools
ENV ANDROID_SDK_ROOT=/opt/android_sdk
ENV ANDROID_HOME=$ANDROID_SDK_ROOT
RUN sudo mkdir -p /opt/sdk
RUN sudo mkdir -p $ANDROID_SDK_ROOT/cmdline-tools/latest
RUN sudo wget https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip -O /opt/archives/android_sdk.zip
RUN sudo unzip /opt/archives/android_sdk.zip -d /opt/sdk
RUN sudo mv /opt/sdk/cmdline-tools/* $ANDROID_SDK_ROOT/cmdline-tools/latest
RUN sudo rm -r /opt/sdk
RUN yes | sudo $ANDROID_SDK_ROOT/cmdline-tools/latest/bin/sdkmanager --licenses
RUN sudo $ANDROID_SDK_ROOT/cmdline-tools/latest/bin/sdkmanager tools platform-tools
ENV PATH=$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools:$PATH

# Gradle
RUN sudo wget https://services.gradle.org/distributions/gradle-8.9-bin.zip -O /opt/archives/gradle-8.9-bin.zip
RUN sudo unzip /opt/archives/gradle-8.9-bin.zip -d /opt
ENV GRADLE_HOME=/opt/gradle-8.9
ENV PATH=$GRADLE_HOME/bin:$PATH

RUN sudo mkdir -p "/etc/docker"
RUN echo "{\"mtu\": 1454}" | sudo tee -a /etc/docker/daemon.json > /dev/null
RUN sudo chown -R runner:runner /opt/android_sdk/
