FROM ubuntu:hadoop6

RUN apt-get install -y curl
ENV MAHOUT_VERSION 0.12.2
ENV MAHOUT_HOME /opt/mahout-${MAHOUT_VERSION}

# Install Mahout
RUN set -x \
    && URL=http://apache-mirror.rbc.ru/pub/apache/mahout/0.12.2/apache-mahout-distribution-0.12.2.tar.gz \
    && curl ${URL} | tar -xzC /opt \
    && mv /opt/apache-mahout-distribution-${MAHOUT_VERSION} ${MAHOUT_HOME}

RUN apt-get update
RUN apt-get install -y bzip2

# Download dataset
RUN curl https://dumps.wikimedia.org/enwiki/latest/enwiki-latest-pages-articles1.xml-p10p30302.bz2 | bunzip2 > /root/wiki
COPY files/*-site.xml ${HADOOP_CONF_DIR}/
COPY files/categories /root/
COPY files/benchmark.sh /root/
RUN chmod +x /root/benchmark.sh && ln -s /root/benchmark.sh /bin/benchmark

# Set JVM heap size to 1GB
ENV HADOOP_CLIENT_OPTS -Xmx1024m
