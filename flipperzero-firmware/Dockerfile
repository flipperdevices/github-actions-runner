FROM summerwind/actions-runner-dind:v2.328.0-ubuntu-22.04

RUN DEBIAN_FRONTEND=noninteractive sudo apt update && sudo apt -y install wget apt-transport-https ca-certificates curl gnupg

RUN curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
RUN echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

RUN curl -fsSL https://cdn.pvs-studio.com/etc/pubkey.txt | gpg --dearmor | sudo tee /usr/share/keyrings/pvs-studio.gpg
RUN echo "deb [arch=amd64 signed-by=/usr/share/keyrings/pvs-studio.gpg] https://cdn.pvs-studio.com/deb viva64-release pvs-studio" | sudo tee /etc/apt/sources.list.d/viva64.list

RUN curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
RUN echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list

RUN DEBIAN_FRONTEND=noninteractive sudo apt update && sudo apt -y install pvs-studio=7.38.97034.608-1 kubectl helm python3-pip
RUN sudo mkdir -p "/etc/docker"
RUN echo "{\"mtu\": 1454}" | sudo tee -a /etc/docker/daemon.json > /dev/null
