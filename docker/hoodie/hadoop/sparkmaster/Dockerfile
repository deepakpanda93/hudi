
#  Licensed to the Apache Software Foundation (ASF) under one
#  or more contributor license agreements.  See the NOTICE file
#  distributed with this work for additional information
#  regarding copyright ownership.  The ASF licenses this file
#  to you under the Apache License, Version 2.0 (the
#  "License"); you may not use this file except in compliance
#  with the License.  You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
#  Unless required by applicable law or agreed to in writing, software
#  distributed under the License is distributed on an "AS IS" BASIS,
#  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#  See the License for the specific language governing permissions and
# limitations under the License.

ARG HADOOP_VERSION=3.4.0
ARG HIVE_VERSION=3.1.3
ARG SPARK_VERSION=3.5.3
FROM apachehudi/hudi-hadoop_${HADOOP_VERSION}-hive_${HIVE_VERSION}-sparkbase_${SPARK_VERSION}:linux-arm64-1.0.2

COPY master.sh /opt/spark
RUN chmod +x /opt/spark/master.sh

COPY startup.sh /opt/startup.sh
RUN chmod +x /opt/startup.sh

ENV SPARK_MASTER_PORT 7077
ENV SPARK_MASTER_WEBUI_PORT 8080
ENV SPARK_JUPYTER_WEBUI_PORT 8888
ENV SPARK_MASTER_LOG /opt/spark/logs


# Install required system libraries
RUN apt-get update && apt-get install -y \
    #build-essential \
    #libffi-dev \
    #libssl-dev \
    #libbz2-dev \
    #libreadline-dev \
    #libsqlite3-dev \
    #zlib1g-dev \
    libfreetype6-dev \
    libpng-dev \
    pkg-config \
    libxft-dev \
    libjpeg-dev \
    curl \
    git \
    wget \
    gfortran \
    libxml2-dev \
    libxslt1-dev \
    libatlas-base-dev

RUN pip install --upgrade pip setuptools wheel

# ---------- Install Jupyter + Python packages ----------
RUN pip install \
    'urllib3<2' \
    jupyterlab \
    pandas \
    matplotlib \
    seaborn \
    ipywidgets \
    findspark

# Create local jars folder and download aws jars into it
RUN mkdir -p jars && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.3.4/hadoop-aws-3.3.4.jar -P ./jars/ && \
    wget https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.12.734/aws-java-sdk-bundle-1.12.734.jar -P ./jars/

EXPOSE 8080 7077 6066 8888

WORKDIR /opt/workspace/notebooks
CMD ["/opt/startup.sh"]
#CMD ["/bin/bash", "/opt/spark/master.sh"]
