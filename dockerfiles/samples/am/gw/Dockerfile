FROM docker.wso2.com/wso2am-universal-gw:4.5.0.0

ARG USER=wso2carbon
ARG USER_HOME=/home/${USER}

ARG WSO2_SERVER_NAME=wso2am-universal-gw
ARG WSO2_SERVER_VERSION=4.5.0
ARG WSO2_SERVER=${WSO2_SERVER_NAME}-${WSO2_SERVER_VERSION}
ARG WSO2_SERVER_HOME=${USER_HOME}/${WSO2_SERVER}

# change user to root for elevated permissions to override inherited non-root user
USER root

# install dependencies
RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends unzip wget \
    && rm -rf /var/lib/apt/lists/* ;

RUN chown wso2carbon:wso2 -R ${WSO2_SERVER_HOME}/

# Copy JDBC MySQL driver
ADD https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.44/mysql-connector-java-5.1.44.jar  ${WSO2_SERVER_HOME}/repository/components/lib

# Copy AM Artifacts
ADD https://github.com/wso2/financial-services-accelerator/releases/download/v4.1.0/org.wso2.financial.services.accelerator.common-4.1.0.jar  ${WSO2_SERVER_HOME}/repository/components/dropins
ADD https://github.com/wso2/financial-services-accelerator/releases/download/v4.1.0/org.wso2.financial.services.accelerator.keymanager-4.1.0.jar  ${WSO2_SERVER_HOME}/repository/components/dropins
ADD https://github.com/wso2/financial-services-accelerator/blob/main/financial-services-accelerator/accelerators/fs-apim/carbon-home/repository/resources/conf/templates/repository/conf/financial-services.xml.j2  ${WSO2_SERVER_HOME}/repository/resources/conf/templates/repository/conf
ADD https://github.com/wso2/financial-services-accelerator/blob/main/financial-services-accelerator/accelerators/fs-apim/carbon-home/repository/conf/financial-services.xml  ${WSO2_SERVER_HOME}/repository/conf

# Copy AM Policy Artifacts
RUN \
    wget -O fs-apim-mediation-artifacts-1.0.0.zip https://github.com/wso2/financial-services-apim-mediation-policies/releases/download/v1.0.0/fs-apim-mediation-artifacts-1.0.0.zip \
    && unzip -d ${USER_HOME}/ fs-apim-mediation-artifacts-1.0.0.zip \
    && rm -f fs-apim-mediation-artifacts-1.0.0.zip

RUN \
    cp -r fs-apim-mediation-artifacts-1.0.0/lib/* ${WSO2_SERVER_HOME}/repository/components/lib \
    && cp -r fs-apim-mediation-artifacts-1.0.0/custom-sequences/* ${WSO2_SERVER_HOME}/repository/deployment/server/synapse-configs/default/sequences \
