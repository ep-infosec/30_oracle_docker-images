# Copyright (c) 2022 Oracle and/or its affiliates.
# Licensed under the Universal Permissive License v 1.0 as shown at https://oss.oracle.com/licenses/upl.
#
# Dockerfile template for Tuxedo 22.1.0.0.0
# 
# Download the following file
#   tuxedo221000_64_Linux_01_x86.zip http://www.oracle.com/technetwork/middleware/tuxedo/downloads/index.html
#
# HOW TO BUILD THIS IMAGE
# -----------------------
# Run:
#      $ ./buildContainerImage.sh -v 22.1.0.0.0 -i tuxedo221000_64_Linux_01_x86.zip -s
#      $ docker tag <image_id_from_above_step> oracle/tuxedo:latest

# Pull base image
FROM oracle/serverjre:8

LABEL maintainer="helptuxedo_ww_grp@oracle.com"

# Common environment variables required for this build (do NOT change)
# --------------------------------------------------------------------
ENV ORACLE_HOME=/u01/oracle \
    JAVA_HOME=/usr/java/default \
    PATH=/usr/java/default/bin:$PATH \
    TUX_PKG=tuxedo221000_64_Linux_01_x86.zip

# Core install doesn't include unzip or gcc, add them
# Setup filesystem and oracle user
# Adjust file permissions, go to /u01 as user 'oracle' to proceed with WLS installation
# ------------------------------------------------------------
RUN yum -y install unzip gcc file hostname which util-linux && rm -rf /var/cache/yum && \
    mkdir -p /u01 && chmod a+xr /u01 && \
    groupadd -g 1000 oracle && useradd -b /u01 -m -g oracle -u 1000 -s /bin/bash oracle 

# Copy packages
# -------------
COPY tuxedo22.1.0.0.0.rsp $TUX_PKG init.sh oraInst.loc /u01/ 
RUN  mv /u01/init.sh /u01/oracle/init.sh && \
     mv /u01/oraInst.loc /etc/ && \
     chown oracle:oracle -R /u01 && \
     chmod +x /u01/oracle/init.sh
USER oracle

# Install Tuxedo
# ------------------------------------------------------------
RUN cd /u01 && \
      mkdir oraInventory && \
      jar xf $TUX_PKG && \
      cd /u01/Disk1/install && \
      chmod -R +x * && \
      ./runInstaller.sh -responseFile /u01/tuxedo22.1.0.0.0.rsp -silent -waitforcompletion && \
      rm -rf /u01/Disk1 \
             /u01/tuxedo22.1.0.0.0.rsp \
             /u01/$TUX_PKG

ENV ORACLE_HOME=/u01/oracle/tuxHome
ENV TUXDIR=/u01/oracle/tuxHome/tuxedo22.1.0.0.0
ENV PATH=$PATH:$TUXDIR/bin \
    LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$TUXDIR/lib

USER oracle
WORKDIR /u01/oracle

# Define ENTRYPOINT. 
ENTRYPOINT ["/u01/oracle/init.sh"]
