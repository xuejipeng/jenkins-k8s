FROM  centos

ARG VERSION=3.16
ARG AGENT_WORKDIR=/root/agent

RUN curl --create-dirs -sSLo /usr/share/jenkins/slave.jar https://repo.jenkins-ci.org/public/org/jenkins-ci/main/remoting/${VERSION}/remoting-${VERSION}.jar \
  && chmod 755 /usr/share/jenkins \
  && chmod 644 /usr/share/jenkins/slave.jar
RUN yum -y install git  libtool-ltdl java-1.8.0-openjdk 
RUN rm -rf /var/cache/yum/x86_64/7/*

ENV AGENT_WORKDIR=${AGENT_WORKDIR}

COPY node /usr/local/
COPY kubectl /usr/bin/kubectl
COPY jenkins-slave /usr/local/bin/jenkins-slave
COPY .kube /root/.kube

COPY hosts /tmp/hosts
RUN mkdir -p -- /lib-override && cp /usr/lib64/libnss_files.so.2 /lib-override
RUN sed -i 's:/etc/hosts:/tmp/hosts:g' /lib-override/libnss_files.so.2
ENV LD_LIBRARY_PATH /lib-override 

RUN mkdir /root/.jenkins && mkdir -p ${AGENT_WORKDIR}
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/node/bin
RUN npm install -g yarn
WORKDIR /root 
ENTRYPOINT ["jenkins-slave"]
