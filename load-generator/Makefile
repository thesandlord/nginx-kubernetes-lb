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

PROJECTID=$(shell gcloud config list project | awk 'FNR ==2 { print $$3 }')
AUTOSCALE_GROUP=$(shell gcloud container clusters describe $(CLUSTER_NAME) --zone $(ZONE) --format yaml | grep -A 1 instanceGroupUrls | awk -F/ 'FNR ==2 {print $$NF}')
CLUSTER_NAME=nginx-demo
SVC=nginxplus
TIME=300
LOAD=100
REQUESTS=1000
RANDOM=$(shell cat /dev/urandom | env LC_CTYPE=C tr -dc 'a-zA-Z0-9' | fold -w 32 | head -n 1)
COMMAND=reverse

all:
	docker run -d --name $(COMMAND)-load rufus/siege-engine siege --concurrent=$(LOAD) --reps=$(REQUESTS) http://$(shell kubectl get services $(SVC) -o template --template='{{range .status.loadBalancer.ingress }}{{ .ip  }}{{end}}')/$(COMMAND)/?str=$(RANDOM)

stop:
	docker stop $(COMMAND)-load
	docker rm $(COMMAND)-load