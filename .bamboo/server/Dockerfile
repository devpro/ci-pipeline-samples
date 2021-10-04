# starts from the base image provided by Atlassian: https://hub.docker.com/r/atlassian/bamboo-server (7.2.2 based on Ubuntu 20.04.1 LTS, codename focal)
FROM atlassian/bamboo-server:7.2.2

# switches to root user for admin commands
USER root

# installs system requirements
RUN apt-get update
RUN apt-get install -y apt-transport-https \
    ca-certificates \
    wget \
    curl \
    gnupg-agent \
    software-properties-common
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
RUN add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
RUN wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb
RUN apt-get update

# installs Docker: https://docs.docker.com/engine/install/ubuntu/
RUN apt-get install -y docker-ce \
    docker-ce-cli \
    containerd.io
RUN usermod -a -G docker bamboo

# installs .NET SDK LTS: https://docs.microsoft.com/en-us/dotnet/core/install/linux-ubuntu
RUN apt-get install -y dotnet-sdk-3.1
RUN apt-get install -y dotnet-sdk-5.0

# switches back to bamboo user
USER bamboo

# updates image entrypoint with commands that can only be ran when the container starts
RUN echo "chown root:docker /var/run/docker.sock" >> /entrypoint.sh
