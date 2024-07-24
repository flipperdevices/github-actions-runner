FROM summerwind/actions-runner-dind:v2.317.0-ubuntu-22.04

RUN DEBIAN_FRONTEND=noninteractive sudo apt update && sudo apt -y install wget apt-transport-https ca-certificates curl gnupg

RUN sudo mkdir -p /opt/archives

# OpenJDK17 temurin
RUN sudo wget https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.12%2B7/OpenJDK17U-jdk_x64_linux_hotspot_17.0.12_7.tar.gz -O /opt/archives/openjdk.tar.gz # https://github.com/adoptium/temurin17-binaries/releases/tag/jdk-17.0.12%2B7
RUN sudo tar -xvf /opt/archives/openjdk.tar.gz -C /opt/
ENV JAVA_HOME=/opt/jdk-17.0.12+7
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

RUN sudo mkdir -p "/etc/docker"
RUN echo "{\"mtu\": 1454}" | sudo tee -a /etc/docker/daemon.json > /dev/null
RUN sudo chown -R runner:runner /opt/android_sdk/

# Optimization block
# Preinstall platform API 34
# https://github.com/flipperdevices/Flipper-Android-App/blob/e895e199f40c81a53f6f65e91b27afa590cb2e43/build-logic/plugins/convention/src/main/kotlin/com/flipperdevices/buildlogic/ApkConfig.kt#L12
RUN sdkmanager --install "platforms;android-34"

# Warmup project
RUN git clone --recurse-submodules https://github.com/flipperdevices/Flipper-Android-App.git -b 1.7.1.1654 /tmp/android-warmup/
RUN cd /tmp/android-warmup/ && ./gradlew assembleDebug -Dorg.gradle.jvmargs="-Xmx4g" && rm -rf /tmp/android-warmup/
