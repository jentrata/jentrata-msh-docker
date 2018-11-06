FROM tomcat:9
MAINTAINER Arran Ubels a.ubels@base2services.com

ENV JDK_VERSION=10

ENV VERSION msh-3.x-SNAPSHOT
ENV BUILD_JOB jentrata-msh-master
ENV BUILD lastBuild

ENV JENTRATA_HOME /opt/jentrata
ENV TOMCAT_HOME $CATALINA_HOME

ENV TOMCAT_USER_NAME jentrata
ENV TOMCAT_USER_PASS jentrata
ENV DB_USER_NAME jentrata
ENV DB_USER_PASS jentrata
ENV DB_HOST_NAME db

# From https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh
COPY wait-for-it.sh /opt/wait-for-it.sh

RUN mkdir -p /opt/jentrata && \
    rm -rf $TOMCAT_HOME/webapps/* && \
    wget https://jentrata.ci.cloudbees.com/job/$BUILD_JOB/$BUILD/artifact/Dist/target/jentrata-$VERSION-tomcat.tar.gz -O - | tar xz -C /opt/jentrata && \
    ln -s $JENTRATA_HOME/webapps/corvus $TOMCAT_HOME/webapps/jentrata && \
    chmod a+x /opt/wait-for-it.sh && \
    mkdir -p /opt/jolokia && \
    wget http://search.maven.org/remotecontent?filepath=org/jolokia/jolokia-jvm/1.6.0/jolokia-jvm-1.6.0-agent.jar -O /opt/jolokia/jolokia-jvm.jar

COPY run.sh /opt/run.sh
COPY ROOT/index.html $TOMCAT_HOME/webapps/ROOT/index.html

EXPOSE 8778

CMD ["/bin/sh", "/opt/run.sh"]
