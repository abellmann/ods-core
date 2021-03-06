FROM openjdk:8-alpine

ENV SONAR_VERSION=7.3 \
    SONARQUBE_HOME=/opt/sonarqube \
    SONARQUBE_JDBC_USERNAME=sonar \
    SONARQUBE_JDBC_PASSWORD=sonar \
    SONARQUBE_JDBC_URL=

# Http port
EXPOSE 9000

RUN adduser -S -G root sonarqube

RUN set -x \
    && apk add --no-cache gnupg unzip \
    && apk add --no-cache libressl wget \
    && apk add --no-cache su-exec \
    && apk add --no-cache bash \
    # pub   2048R/D26468DE 2015-05-25
    #       Key fingerprint = F118 2E81 C792 9289 21DB  CAB4 CFCA 4A29 D264 68DE
    # uid                  sonarsource_deployer (Sonarsource Deployer) <infra@sonarsource.com>
    # sub   2048R/06855C1D 2015-05-25
    # && if [[ $HTTP_PROXY != "" ]]; then gpg --keyserver ha.pool.sks-keyservers.net --keyserver-options "timeout=10 http-proxy=$http_proxy" --recv-keys F1182E81C792928921DBCAB4CFCA4A29D26468DE; else gpg --keyserver ha.pool.sks-keyservers.net --recv-keys F1182E81C792928921DBCAB4CFCA4A29D26468DE; fi \
    && mkdir -p /opt \
    && cd /opt \
    && wget -O sonarqube.zip --no-verbose https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip \
    # && wget -O sonarqube.zip.asc --no-verbose https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip.asc \
    # && gpg --batch --verify sonarqube.zip.asc sonarqube.zip \
    && unzip sonarqube.zip \
    && mv sonarqube-$SONAR_VERSION sonarqube \
    && chown -R sonarqube:root sonarqube \
    && chmod -R g+w sonarqube \
    && rm sonarqube.zip* \
    && rm -rf $SONARQUBE_HOME/bin/* \
    && mkdir -p /opt/configuration/sonarqube \
    && chown -R sonarqube:root /opt/configuration/sonarqube

USER sonarqube

VOLUME "$SONARQUBE_HOME/data"

WORKDIR $SONARQUBE_HOME

COPY run.sh $SONARQUBE_HOME/bin/
COPY plugins /opt/configuration/sonarqube/plugins

ENTRYPOINT ["./bin/run.sh"]
