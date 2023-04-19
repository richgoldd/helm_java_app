FROM openjdk:11.0.5-jdk
ARG SOURCEJAR=/home/runner/work/helm_java_app/helm_java_app/target

COPY ${SOURCEJAR}/*.jar app.jar
ENTRYPOINT java -jar app.jar
