FROM maven:3-jdk-8 as build-ctakes

RUN git clone https://github.com/apache/ctakes ctakes

COPY customDictionary.xml /ctakes/ctakes-web-rest/src/main/resources/org/apache/ctakes/dictionary/lookup/fast/
COPY pom.xml /ctakes

# This version of the default piper comments out a memory-intensive negation module. If you need to run
# negation detection, then comment out this line.
COPY Default.piper /ctakes/ctakes-web-rest/src/main/resources/pipers/

RUN cd ctakes && mvn compile -DskipTests && mvn install -pl '!ctakes-distribution'  -DskipTests

FROM tomcat:9-jdk8

# COPY --from=build-ctakes /ctakes /ctakes
# RUN cp /ctakes/ctakes-web-rest/target/ctakes-web-rest.war /usr/local/tomcat/webapps/
COPY --from=build-ctakes /ctakes/ctakes-web-rest/target/ctakes-web-rest.war /usr/local/tomcat/webapps/

ENV CTAKES_HOME=/ctakes


EXPOSE 8080

CMD ["catalina.sh", "run"]
