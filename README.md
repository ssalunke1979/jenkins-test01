# jenkins-test01

#Jenkins on docker

##########build docker image
FROM jenkins/jenkins:lts
USER root
RUN apt-get update && \
    apt-get -y install apt-transport-https \
         ca-certificates curl gnupg2 \
         software-properties-common && \
    curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && \
    add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
      $(lsb_release -cs) \
      stable" && \
    apt-get update && \
    apt-get -y install docker-ce
USER jenkins

###############

docker build -t jenkins:v1 .

docker run -p 8080:8080 -p 50000:50000 -d -v /jenkins/home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock jenkins:v1

#run docker image using sudo
docker exec -u 0 -it 557c4a24ff3f bash

#run docker from normal user 
sudo chmod 666 /var/run/docker.sock
