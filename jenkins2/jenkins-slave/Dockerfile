FROM centos:7
MAINTAINER Mike Eder

ARG remove_packages="docker docker-common docker-selinux docker-engine"
ARG install_packages="git wget openssh-server java-1.8.0-openjdk sudo yum-utils device-mapper-persistent-data lvm2"
ARG docker_repo="https://download.docker.com/linux/centos/docker-ce.repo"
ARG docker_group=998

# Install Packages
RUN yum remove $remove_packages \
    && yum -v -y install $install_packages \
	&& yum clean all

# Install Docker-CE
RUN yum-config-manager --add-repo $docker_repo \
    && yum install docker-ce -y

# Create Jenkins User
RUN useradd jenkins -m -s /bin/bash

# Add Jenkins user to Docker group
RUN usermod --gid $docker_group jenkins
RUN usermod -a -G docker jenkins

RUN chown -R jenkins /home/jenkins
RUN chgrp -R docker /home/jenkins

# Add the jenkins user to sudoers
RUN echo "jenkins    ALL=(ALL)    ALL" >> etc/sudoers

# Set Name Servers
COPY /files/resolv.conf /etc/resolv.conf

