FROM summerwind/actions-runner-dind:v2.317.0-ubuntu-22.04

RUN DEBIAN_FRONTEND=noninteractive sudo apt update && sudo apt -y install wget apt-transport-https ca-certificates curl gnupg

RUN curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
RUN echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

RUN wget -q -O - https://cdn.pvs-studio.com/etc/pubkey.txt | sudo apt-key add -
RUN sudo wget -O /etc/apt/sources.list.d/viva64.list https://cdn.pvs-studio.com/etc/viva64.list

RUN curl https://baltocdn.com/helm/signing.asc | sudo gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list

RUN DEBIAN_FRONTEND=noninteractive sudo apt update && sudo apt -y install pvs-studio=7.31.81895.392-1 kubectl helm python3-pip

RUN sudo mkdir -p "/etc/docker"
RUN echo "{\"mtu\": 1454}" | sudo tee -a /etc/docker/daemon.json > /dev/null
