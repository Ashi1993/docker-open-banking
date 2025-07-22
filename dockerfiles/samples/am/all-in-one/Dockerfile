FROM docker.wso2.com/wso2am:4.5.0.0

ARG USER=wso2carbon

ARG USER_HOME=/home/${USER}
ARG WSO2_SERVER_NAME=wso2am
ARG WSO2_SERVER_VERSION=4.5.0
ARG WSO2_SERVER=${WSO2_SERVER_NAME}-${WSO2_SERVER_VERSION}
ARG WSO2_SERVER_HOME=${USER_HOME}/${WSO2_SERVER}

USER root

# Copy JDBC MySQL driver
ADD https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.44/mysql-connector-java-5.1.44.jar  ${WSO2_SERVER_HOME}/repository/components/lib

# Copy AM Artifacts
ADD https://github.com/wso2/financial-services-accelerator/releases/download/v4.1.0/org.wso2.financial.services.accelerator.common-4.1.0.jar  ${WSO2_SERVER_HOME}/repository/components/dropins
ADD https://github.com/wso2/financial-services-accelerator/releases/download/v4.1.0/org.wso2.financial.services.accelerator.keymanager-4.1.0.jar  ${WSO2_SERVER_HOME}/repository/components/dropins

COPY --chown=wso2carbon:wso2 financial-services.xml.j2 ${WSO2_SERVER_HOME}/repository/resources/conf/templates/repository/conf
COPY --chown=wso2carbon:wso2 financial-services.xml ${WSO2_SERVER_HOME}/repository/conf

# change directory rights
RUN chown wso2carbon:wso2 -R ${WSO2_SERVER_HOME}/

# set the non-root user
USER ${USER}