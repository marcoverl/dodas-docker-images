#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
#ARG java_image_tag=8-jre-slim

FROM ubuntu:focal

WORKDIR /

ARG spark_uid=185

# Before building the docker image, first build and make a Spark distribution following
# the instructions in http://spark.apache.org/docs/latest/building-spark.html.
# If this docker file is being used in the context of building your images from a Spark
# distribution, the docker build command should be invoked from the top level directory
# of the Spark distribution. E.g.:
# docker build -t spark:latest -f kubernetes/dockerfiles/spark/Dockerfile .

RUN set -ex && \
#    sed -i 's/http:/https:/g' /etc/apt/sources.list && \
    ln -s /lib /lib64 && \
    apt-get update && DEBIAN_FRONTEND="noninteractive" TZ="Europe/Rome" apt install -y openjdk-8-jdk bash tini libc6 libpam-modules krb5-user libnss3 wget python3 python3-pip && \
    rm /bin/sh && \
    ln -sv /bin/bash /bin/sh && \
    echo "auth required pam_wheel.so use_uid" >> /etc/pam.d/su && \
    chgrp root /etc/passwd && chmod ug+rw /etc/passwd && \
    rm -rf /var/cache/apt/* && \
    cd /usr/local/ && wget https://downloads.apache.org/spark/spark-3.0.1/spark-3.0.1-bin-hadoop3.2.tgz && \
    tar -xzvf spark-3.0.1-bin-hadoop3.2.tgz && \
    mv spark-3.0.1-bin-hadoop3.2 spark && \
    rm -f spark-3.0.1-bin-hadoop3.2.tgz && \
    mkdir -p /usr/local/spark/work-dir

RUN pip3 install --upgrade pip setuptools && \
    rm -r /root/.cache && rm -rf /var/cache/apt/* && \
    ln -s /usr/bin/python3 /usr/bin/python

# Add S3 jars
ADD https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.11.878/aws-java-sdk-bundle-1.11.878.jar /usr/local/spark/jars/
ADD https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.2.0/hadoop-aws-3.2.0.jar /usr/local/spark/jars/

ENV SPARK_HOME /usr/local/spark

WORKDIR /usr/local/spark/work-dir
RUN chmod g+w /usr/local/spark/work-dir && chmod go+r /usr/local/spark/jars/*

ENTRYPOINT [ "/usr/local/spark/kubernetes/dockerfiles/spark/entrypoint.sh" ]

# Specify the User that the actual main process will run as
USER ${spark_uid}
