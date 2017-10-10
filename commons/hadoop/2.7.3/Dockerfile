FROM arm64v8/ubuntu:16.04


# Install dependencies
RUN apt-get update -y && apt-get install -y --no-install-recommends \
      curl \
      openjdk-8-jdk \
      rsync \
      ssh \
    && rm -rf /var/lib/apt/lists/*

ENV HADOOP_VERSION 2.7.4
ENV HADOOP_PREFIX /opt/hadoop-${HADOOP_VERSION}
ENV HADOOP_CONF_DIR ${HADOOP_PREFIX}/etc/hadoop
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-arm64

# Install Hadoop
RUN set -x \
    && URL=http://mirror.jax.hugeserver.com/apache/hadoop/common/hadoop-2.7.4/hadoop-2.7.4.tar.gz \
    && curl ${URL} | tar -xzC /opt

# Configure Hadoop
RUN sed -i "s:JAVA_HOME=.*:JAVA_HOME=${JAVA_HOME}:" ${HADOOP_PREFIX}/etc/hadoop/hadoop-env.sh

# Configure ssh
RUN ssh-keygen -t rsa -P '' -f /root/.ssh/id_rsa \
    && cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys \
    && chmod 0600 /root/.ssh/authorized_keys \
    && echo "Host *\n  StrictHostKeyChecking no" > /root/.ssh/config

COPY files/*-site.xml ${HADOOP_PREFIX}/etc/hadoop/
COPY files/entrypoint.sh files/example_benchmark.sh /root/
RUN chmod +x /root/entrypoint.sh /root/example_benchmark.sh

ENTRYPOINT ["/root/entrypoint.sh"]
