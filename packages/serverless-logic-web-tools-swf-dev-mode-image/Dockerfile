# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#  http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.

ARG BASE_IMAGE_TAG

FROM --platform=linux/amd64 $BASE_IMAGE_TAG

ENV PATH="${PATH}:/usr/share/maven/bin"

USER root
RUN chown kogito /home/kogito/.m2
USER kogito

COPY --chown=kogito:root dist-dev/quarkus-app/ /home/kogito/serverless-logic-web-tools-swf-deployment-quarkus-app/
COPY --chown=kogito:root dist-dev/quarkus-app-m2/ /home/kogito/.m2/repository

WORKDIR /home/kogito/serverless-logic-web-tools-swf-deployment-quarkus-app/

RUN rm -rf src/test/ && \
  mvn clean package \
  quarkus:go-offline \
  -B \
  -ntp \
  -Dmaven.test.skip \
  -Dquarkus-profiles=build,dev

USER root
RUN chmod -R 775 /home/kogito
USER kogito

ENTRYPOINT mvn quarkus:dev \
  quarkus:go-offline \
  -nsu \
  -Ddebug=false \
  -Dmaven.repo.local=/home/kogito/.m2/repository \
  -Dquarkus.http.port=8080
