FROM openjdk:8-jdk-alpine

LABEL maintainer "Andrés Correa Casablanca <andreu@adsmurai.com>"

ENV HOME /user

ARG IMPLY_VERSION=2.4.7
ARG IMPLY_DOWNLOAD_SHA256=b1e44e61d6db33fcbd60354a0e5cc63871191eadd2d82bdc98a1a1974604f214

RUN set -o errexit -o nounset                                                                                          \
    && echo "Installing dependencies"                                                                                  \
	&& apk add --no-cache                                                                                              \
	    bash                                                                                                           \
	    nodejs                                                                                                         \
		perl                                                                                                           \
		python                                                                                                         \
		                                                                                                               \
	&& echo "Downloading Imply Platform package"                                                                       \
	&& wget -O imply.tar.gz "https://static.imply.io/release/imply-${IMPLY_VERSION}.tar.gz"                            \
                                                                                                                       \
    && echo "Checking download hash"                                                                                   \
	&& echo "${IMPLY_DOWNLOAD_SHA256}  imply.tar.gz" | sha256sum -c                                                    \
                                                                                                                       \
    && echo "Extracting Imply Platform package"                                                                        \
	&& tar -xzf imply.tar.gz                                                                                           \
                                                                                                                       \
    && echo "Relocating Imply Platform binaries"                                                                       \
    && mkdir -p /opt                                                                                                   \
    && mv /imply-${IMPLY_VERSION} /opt/imply                                                                           \
                                                                                                                       \
    && echo "Removing Imply UI / Pivot for license compliance"                                                         \
    && rm -rf /opt/imply/dist/imply-ui                                                                                 \
    && unlink /opt/imply/bin/run-pivot                                                                                 \
    && unlink /opt/imply/bin/run-pivot-quickstart                                                                      \
    && rm /opt/imply/bin/run-imply-ui                                                                                  \
    && rm /opt/imply/bin/run-imply-ui-quickstart                                                                       \
    && rm -rf /opt/imply/conf/pivot                                                                                    \
    && rm -rf /opt/imply/conf-quickstart/pivot                                                                         \
    && rm /opt/imply/quickstart/pivot-settings.json                                                                    \
                                                                                                                       \
    && echo "Cleaning build binaries"                                                                                  \
    && rm imply.tar.gz                                                                                                 ;

# Using modified versions of settings files to make easier deployments
COPY conf/supervise/quickstart.conf                           /opt/imply/conf/supervise/quickstart.conf

COPY conf/druid/_common/common.runtime.properties  /opt/imply/conf/druid/_common/common.runtime.properties
COPY conf/druid/broker/jvm.config                  /opt/imply/conf/druid/broker/jvm.config
COPY conf/druid/broker/runtime.properties          /opt/imply/conf/druid/broker/runtime.properties
COPY conf/druid/coordinator/jvm.config             /opt/imply/conf/druid/coordinator/jvm.properties
COPY conf/druid/historical/jvm.config              /opt/imply/conf/druid/historical/jvm.config
COPY conf/druid/historical/runtime.properties      /opt/imply/conf/druid/historical/runtime.properties
COPY conf/druid/middleManager/jvm.config           /opt/imply/conf/druid/middleManager/jvm.config
COPY conf/druid/middleManager/runtime.properties   /opt/imply/conf/druid/middleManager/runtime.properties
COPY conf/druid/overlord/jvm.config                /opt/imply/conf/druid/overlord/jvm.config
COPY conf/zk/zoo.cfg                               /opt/imply/conf/zk/zoo.cfg

COPY quickstart-conf/druid/_common/common.runtime.properties  /opt/imply/quickstart-conf/druid/_common/common.runtime.properties
COPY quickstart-conf/druid/broker/jvm.config                  /opt/imply/quickstart-conf/druid/broker/jvm.config
COPY quickstart-conf/druid/broker/runtime.properties          /opt/imply/quickstart-conf/druid/broker/runtime.properties
COPY quickstart-conf/druid/coordinator/jvm.config             /opt/imply/quickstart-conf/druid/coordinator/jvm.properties
COPY quickstart-conf/druid/historical/jvm.config              /opt/imply/quickstart-conf/druid/historical/jvm.config
COPY quickstart-conf/druid/historical/runtime.properties      /opt/imply/quickstart-conf/druid/historical/runtime.properties
COPY quickstart-conf/druid/middleManager/jvm.config           /opt/imply/quickstart-conf/druid/middleManager/jvm.config
COPY quickstart-conf/druid/middleManager/runtime.properties   /opt/imply/quickstart-conf/druid/middleManager/runtime.properties
COPY quickstart-conf/druid/overlord/jvm.config                /opt/imply/quickstart-conf/druid/overlord/jvm.config
COPY quickstart-conf/zk/zoo.cfg                               /opt/imply/quickstart-conf/zk/zoo.cfg

WORKDIR /opt/imply

VOLUME /var/imply

# We don't expose 9095 because that's the Pivot port
EXPOSE 1527 2181 8081 8082 8083 8090 8091 8100 8101 8102 8103 8104 8105 8106 8107 8108 8109 8110 8200

CMD ["/opt/imply/bin/supervise", "-c", "/opt/imply/conf/supervise/quickstart.conf"]
