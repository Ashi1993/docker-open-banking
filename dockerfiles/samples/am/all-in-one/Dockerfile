FROM docker.wso2.com/wso2am:4.5.0.0

ARG USER=wso2carbon

ARG USER_HOME=/home/${USER}
ARG WSO2_SERVER_NAME=wso2am
ARG WSO2_SERVER_VERSION=4.5.0
ARG WSO2_SERVER=${WSO2_SERVER_NAME}-${WSO2_SERVER_VERSION}
ARG WSO2_SERVER_HOME=${USER_HOME}/${WSO2_SERVER}

USER root

# For OpenShift
#RUN chgrp -R 0 ${USER_HOME} && chmod -R g=u ${USER_HOME} \
#    && chgrp -R 0 ${WSO2_SERVER_HOME} && chmod -R g=u ${WSO2_SERVER_HOME} \
#    && chgrp -R 0 ${USER_HOME}/solr && chmod -R g=u ${USER_HOME}/solr

# install dependencies
RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends unzip wget \
    && rm -rf /var/lib/apt/lists/* ;

# Copy JDBC MySQL driver
ADD https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.44/mysql-connector-java-5.1.44.jar  ${WSO2_SERVER_HOME}/repository/components/lib

# Copy AM Artifacts
ADD https://github.com/wso2/financial-services-accelerator/releases/download/v4.1.0/org.wso2.financial.services.accelerator.common-4.1.0.jar  ${WSO2_SERVER_HOME}/repository/components/dropins
ADD https://github.com/wso2/financial-services-accelerator/releases/download/v4.1.0/org.wso2.financial.services.accelerator.keymanager-4.1.0.jar  ${WSO2_SERVER_HOME}/repository/components/dropins

COPY --chown=wso2carbon:wso2 financial-services.xml.j2 ${WSO2_SERVER_HOME}/repository/resources/conf/templates/repository/conf
COPY --chown=wso2carbon:wso2 financial-services.xml ${WSO2_SERVER_HOME}/repository/conf
COPY --chown=wso2carbon:wso2 api#fs#backend.war ${WSO2_SERVER_HOME}/repository/deployment/server/webapps

# Copy AM Policy Artifacts
RUN \
    wget -O fs-apim-mediation-artifacts-1.0.0.zip https://github.com/wso2/financial-services-apim-mediation-policies/releases/download/v1.0.0/fs-apim-mediation-artifacts-1.0.0.zip \
    && unzip -d ${USER_HOME}/ fs-apim-mediation-artifacts-1.0.0.zip \
    && rm -f fs-apim-mediation-artifacts-1.0.0.zip

RUN \
    cp -r fs-apim-mediation-artifacts-1.0.0/lib/* ${WSO2_SERVER_HOME}/repository/components/lib \
    && cp -r fs-apim-mediation-artifacts-1.0.0/custom-sequences/* ${WSO2_SERVER_HOME}/repository/deployment/server/synapse-configs/default/sequences

# change directory rights
RUN chown wso2carbon:wso2 -R ${WSO2_SERVER_HOME}/

# set the non-root user
USER ${USER}