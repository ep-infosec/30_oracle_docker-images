# Copyright (c) 2022 Oracle and/or its affiliates.
# Licensed under the Universal Permissive License v 1.0 as shown at https://oss.oracle.com/licenses/upl.

# Pull Tuxedo base image
FROM oracle/tuxedo:latest

LABEL maintainer="helptuxedo_ww_grp@oracle.com"

COPY ws_svr_runme.sh stop_server.sh serverws.c simpserv.c openssl.cnf /u01/oracle/

USER root
RUN yum -y install openssl && rm -rf /var/cache/yum && \
    chown oracle:oracle -R /u01/oracle/ && \
    chmod +x /u01/oracle/ws_svr_runme.sh &&  \
    chmod +x /u01/oracle/stop_server.sh

USER oracle
WORKDIR /u01/oracle

# Define ENTRYPOINT. 
ENTRYPOINT ["/u01/oracle/ws_svr_runme.sh"]

