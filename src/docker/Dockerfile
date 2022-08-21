FROM @from.image@ as builder

WORKDIR /@project.name@

ADD ./ /@project.name@

ARG JAR_FILE=*.jar
ADD ${JAR_FILE} app.jar

RUN java -Djarmode=layertools -jar app.jar extract

ENV JAVA_OPTS=""
ENV SPRING_PROFILES_ACTIVE=""
ENV CONFIG_SERVER=""

## Stage 2 Layers
FROM @from.image@ as final

WORKDIR /@project.name@

VOLUME /tmp

COPY --from=builder @project.name@/dependencies/ ./
COPY --from=builder @project.name@/spring-boot-loader/ ./
COPY --from=builder @project.name@/snapshot-dependencies/ ./
COPY --from=builder @project.name@/application/ ./

ENV TZ="America/Sao_Paulo"
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

ENTRYPOINT ["java","-jar","app.jar","--spring.profiles.active=${SPRING_PROFILES_ACTIVE}"]

