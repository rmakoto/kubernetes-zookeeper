FROM ubuntu:16.04 
ENV ZK_USER=zookeeper \
ZK_DATA_DIR=/var/lib/zookeeper/data \
ZK_DATA_LOG_DIR=/var/lib/zookeeper/data/log \
ZK_LOG_DIR=/var/log/zookeeper \
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

ARG ZK_DIST=zookeeper-3.5.5
RUN set -x \
    && apt-get update \
    && apt-get install -y openjdk-8-jre-headless wget netcat-openbsd \
	&& wget -q "http://www.apache.org/dist/zookeeper/$ZK_DIST/apache-$ZK_DIST-bin.tar.gz" \
    && mv apache-$ZK_DIST-bin.tar.gz $ZK_DIST.tar.gz \
    && export GNUPGHOME="$(mktemp -d)" \
    && tar -xzf "$ZK_DIST.tar.gz" -C /opt \
    && mv /opt/apache-$ZK_DIST-bin /opt/$ZK_DIST \
    && rm -r "$GNUPGHOME" "$ZK_DIST.tar.gz" \
    && ln -s /opt/$ZK_DIST /opt/zookeeper \
    && rm -rf /opt/zookeeper/LICENSE.txt \
    /opt/zookeeper/README.txt \
    /opt/zookeeper/NOTICE.txt \
    /opt/zookeeper/README_packaging.txt \
    /opt/zookeeper/docs \
    /opt/zookeeper/src \
	&& apt-get autoremove -y wget \
	&& rm -rf /var/lib/apt/lists/*

#Copy configuration generator script to bin
COPY scripts /opt/zookeeper/bin/

# Create a user for the zookeeper process and configure file system ownership 
# for necessary directories and symlink the distribution as a user executable
RUN set -x \
	&& useradd $ZK_USER \
    && [ `id -u $ZK_USER` -eq 1000 ] \
    && [ `id -g $ZK_USER` -eq 1000 ] \
    && mkdir -p $ZK_DATA_DIR $ZK_DATA_LOG_DIR $ZK_LOG_DIR /usr/share/zookeeper /tmp/zookeeper /usr/etc/ \
	&& chown -R "$ZK_USER:$ZK_USER" /opt/$ZK_DIST $ZK_DATA_DIR $ZK_LOG_DIR $ZK_DATA_LOG_DIR /tmp/zookeeper \
	&& ln -s /opt/zookeeper/conf/ /usr/etc/zookeeper \
	&& ln -s /opt/zookeeper/bin/* /usr/bin \
	&& ln -s /opt/zookeeper/lib/* /usr/share/zookeeper 
