FROM jenkins
COPY plugins.txt /usr/share/jenkins/plugins.txt
COPY kubectl /usr/bin/
RUN apt-get update && apt-get install libltdl7 && apt-get clean
RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt
