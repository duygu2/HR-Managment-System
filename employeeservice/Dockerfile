FROM maven:3.8.3-openjdk-17 as build
WORKDIR /app
ARG GITHUB_USERNAME
ARG GITHUB_TOKEN


RUN mkdir -p /root/.m2 && \
    echo "<?xml version='1.0' encoding='UTF-8'?><settings xmlns='http://maven.apache.org/SETTINGS/1.2.0' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance' xsi:schemaLocation='http://maven.apache.org/SETTINGS/1.2.0 https://maven.apache.org/xsd/settings-1.2.0.xsd'><servers><server><id>github</id><username>${GITHUB_USERNAME}</username><password>${GITHUB_TOKEN}</password></server></servers></settings>" > /root/.m2/settings.xml

COPY pom.xml .
COPY src ./src
RUN mvn clean package -s /root/.m2/settings.xml -DskipTests

FROM openjdk:17-jdk
COPY --from=build /app/target/employeeservice-0.0.1-SNAPSHOT.jar /app/employeeservice.jar
# EXPOSE 8082
CMD java -jar /app/employeeservice.jar