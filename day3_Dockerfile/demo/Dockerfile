FROM java:8-jdk-alpine
COPY ./target/demo.war /usr/app/
WORKDIR /usr/app
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "demo.war"]

