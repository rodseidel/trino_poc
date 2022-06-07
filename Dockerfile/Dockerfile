ARG TRINO_VERSION=384

FROM trinodb/trino:${TRINO_VERSION}

USER root

RUN yum update \
  && yum install -y httpd-tools
