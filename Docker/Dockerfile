FROM alpine/git as clone
RUN mkdir -p /root/.ssh
# ADD http://172.17.0.2:80/id_rsa /root/.ssh/id_rsa
COPY config /root/.ssh/config
RUN chmod 700 -R /root/.ssh
ARG url
WORKDIR /app
RUN git clone -b LinuxBranch ${url}

FROM maven as archive
ARG project_path
WORKDIR /app
COPY --from=clone /app/${project_path} /app
# ADD http://172.17.0.2:80/settings.xml $MAVEN_HOME/conf/
RUN mvn clean install compile war:war

FROM tomcat:8.5
COPY --from=archive /app/target/infybank.war /usr/local/tomcat/webapps
WORKDIR /usr/local/tomcat
RUN chmod +x bin/catalina.sh
CMD ["bin/catalina.sh", "run"]

