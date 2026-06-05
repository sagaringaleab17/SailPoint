FROM tomcat:jdk11-openjdk-slim

MAINTAINER Steffen Sperling <steffen.sperling@ventum.com>

ENV IIQ_VERSION=8.3

# Install dependencies
RUN apt-get update && \
    apt-get install -y apt-utils default-mysql-client

# Add admin/admin user
ADD tomcat-users.xml /usr/local/tomcat/conf/

# Configure Tomcat Manager
RUN mkdir -p /usr/local/tomcat/conf/Catalina/localhost
ADD manager.xml /usr/local/tomcat/conf/Catalina/localhost/

# Add IdentityIQ
COPY src/identityiq-${IIQ_VERSION}.zip /tmp/

RUN cd /tmp && \
    jar -xf identityiq-${IIQ_VERSION}.zip && \
    mkdir -p /usr/local/tomcat/webapps/identityiq && \
    cd /usr/local/tomcat/webapps/identityiq && \
    jar -xf /tmp/identityiq.war && \
    chmod +x /usr/local/tomcat/webapps/identityiq/WEB-INF/bin/iiq && \
    rm -f /tmp/identityiq.war

# Default landing page
RUN mkdir -p /usr/local/tomcat/webapps/ROOT
COPY index.html /usr/local/tomcat/webapps/ROOT/

# Startup script
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

EXPOSE 8080
EXPOSE 8009

VOLUME ["/usr/local/tomcat/webapps"]

WORKDIR /usr/local/tomcat

# Launch IdentityIQ
CMD ["/entrypoint.sh", "run"]
