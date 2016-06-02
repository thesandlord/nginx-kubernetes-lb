#	Copyright 2016, Google, Inc.
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM debian:8.3

RUN apt-get update && apt-get -y install wget

RUN mkdir -p /etc/ssl/nginx && wget -P /etc/ssl/nginx https://cs.nginx.com/static/files/CA.crt

COPY nginx-repo.key /etc/ssl/nginx/nginx-repo.key
COPY nginx-repo.crt /etc/ssl/nginx/nginx-repo.crt

RUN wget http://nginx.org/keys/nginx_signing.key && apt-key add nginx_signing.key

RUN apt-get -y install apt-transport-https libcurl3-gnutls lsb-release
RUN printf "deb https://plus-pkgs.nginx.com/debian `lsb_release -cs` nginx-plus\n" | tee /etc/apt/sources.list.d/nginx-plus.list
RUN wget -P /etc/apt/apt.conf.d https://cs.nginx.com/static/files/90nginx

RUN apt-get update && apt-get -y install nginx-plus

RUN mkdir /data
COPY index.html /data/index.html

COPY nginx.conf /etc/nginx/conf.d/backend.conf
RUN rm /etc/nginx/conf.d/default.conf

CMD ["nginx", "-g", "daemon off;"]