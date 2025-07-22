# ----------------------------------------------------------------------------------------
#
# Copyright (c) 2025, WSO2 LLC. (https://www.wso2.com). All Rights Reserved.
#
# This software is the property of WSO2 LLC. and its suppliers, if any.
# Dissemination of any information or reproduction of any material contained
# herein in any form is strictly forbidden, unless permitted by WSO2 expressly.
# You may not alter or remove any copyright or other notice from copies of this content.
#
# ----------------------------------------------------------------------------------------


# set base Docker image to WSO2 Identity Server Alpine Docker image
FROM docker.wso2.com/wso2is:7.1.0.0

# set Docker image build arguments
# build arguments for user/group configurations
ARG USER=wso2carbon
ARG USER_ID=802
ARG USER_GROUP=wso2
ARG USER_GROUP_ID=802
ARG USER_HOME=/home/${USER}
# build arguments for WSO2 product installation
ARG WSO2_SERVER_NAME=wso2is
# declaring again BASE_PRODUCT_VERSION ARG to use in build stage
ARG BASE_PRODUCT_VERSION=7.1.0
ARG WSO2_FS_ACCELERATOR_NAME=wso2-fsiam-accelerator
ARG WSO2_FS_ACCELERATOR_VERSION=4.0.0
ARG WSO2_SERVER=${WSO2_SERVER_NAME}-${BASE_PRODUCT_VERSION}
ARG WSO2_FS_ACCELERATOR=${WSO2_FS_ACCELERATOR_NAME}-${WSO2_FS_ACCELERATOR_VERSION}
ARG WSO2_SERVER_HOME=${USER_HOME}/${WSO2_SERVER}
ARG WSO2_FS_ACCELERATOR_DIR=${WSO2_SERVER_HOME}/${WSO2_FS_ACCELERATOR}
ARG WSO2_OB_ACCELERATOR_DIST_URL

ENV ENV=${USER_HOME}"/.ashrc"

# change user to root for elevated permissions to override inherited non-root user
USER root

# add Open Banking artifacts to base product
RUN \
    wget -O ${WSO2_FS_ACCELERATOR}.zip "${WSO2_OB_ACCELERATOR_DIST_URL}" \
    && unzip -d ${WSO2_SERVER_HOME}/ ${WSO2_FS_ACCELERATOR}.zip \
    && rm -f ${WSO2_FS_ACCELERATOR}.zip

# set configurations
# read deployment.toml file
ARG DEPLOYMENT_TOML_FILE=${WSO2_FS_ACCELERATOR_DIR}/repository/resources/deployment.toml
RUN \
    cp ${WSO2_FS_ACCELERATOR_DIR}/repository/resources/${WSO2_SERVER}-deployment.toml ${DEPLOYMENT_TOML_FILE} \
# copy deployment.toml file to original product pack location
    && cp ${DEPLOYMENT_TOML_FILE} ${WSO2_SERVER_HOME}/repository/conf/ \
    && rm ${DEPLOYMENT_TOML_FILE}

# copy the WSO2 FSIAM accelerator artifacts on top of WSO2 IS base product
# remove old open banking artifacts from base product
RUN \
    find ${WSO2_SERVER_HOME}/repository/components/dropins -name "org.wso2.financial.services.accelerator.*" -exec rm -rf {} \; \
    && find ${WSO2_SERVER_HOME}/repository/components/lib -name "org.wso2.financial.services.accelerator.*" -exec rm -rf {} \;

RUN \
    cp -r ${WSO2_FS_ACCELERATOR_DIR}/carbon-home/* ${WSO2_SERVER_HOME}/

ADD https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.44/mysql-connector-java-5.1.44.jar  ${WSO2_SERVER_HOME}/repository/components/lib

# change directory rights
RUN chown wso2carbon:wso2 -R ${WSO2_SERVER_HOME}/

# set the non-root user
USER ${USER_ID}

# set MOTD login message for open banking
RUN echo ${MOTD} > "${ENV}"

# expose ports
EXPOSE 9446

# For OpenShift
#USER root
#RUN chgrp -R root /home/wso2carbon/wso2is-* && chmod -R g+rwX /home/wso2carbon/wso2is-*
#USER wso2carbon

# initiate container and start WSO2 Carbon server
ENTRYPOINT ["/home/wso2carbon/docker-entrypoint.sh"]
