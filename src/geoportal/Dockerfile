FROM tomcat:9
MAINTAINER Bhuvana reference to Mathias Kühn 

# Install wget and install/updates certificates
RUN apt-get update \
 && apt-get install -y -q --no-install-recommends \
    ca-certificates \
    git \
    maven \
    #openjdk-8-jdk="$JAVA_DEBIAN_VERSION" \
    #ca-certificates-java="$CA_CERTIFICATES_JAVA_VERSION" \
    openjdk-8-jdk \
    ca-certificates-java \
 && apt-get clean \
 && rm -r /var/lib/apt/lists/*

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

# Install geoportal web application
ENV GPTURL=https://github.com/Bhuvana2509/geoportal_server/archive/v1.2.zip

ADD $GPTURL /tmp/geoportal.zip

WORKDIR $CATALINA_HOME/webapps/
RUN unzip /tmp/geoportal.zip
RUN rm /tmp/geoportal.zip

COPY tomcat-users.xml $CATALINA_HOME/conf

# Install harvester application
ENV GITURL=https://github.com/Esri/geoportal-server-harvester.git

WORKDIR /tmp
RUN git clone ${GITURL} harvester
WORKDIR /tmp/harvester
RUN mvn package -DskipTests

WORKDIR $CATALINA_HOME/webapps/

RUN cp /tmp/harvester/geoportal-application/geoportal-harvester-war/target/*.war harvester.war
