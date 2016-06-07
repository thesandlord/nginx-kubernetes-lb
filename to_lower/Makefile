# 	Copyright 2016, Google, Inc.
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at

#    http://www.apache.org/licenses/LICENSE-2.0

# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

PROJECT=$(shell gcloud config list project | awk 'FNR ==2 { print $$3 }')
TAG=gcr.io/$(PROJECT)/$(NAME)
NAME=lower

all: clean build run
	
build:
	docker build -t $(TAG) .

run:
	docker run -d -p 80:80 --name=$(NAME) $(TAG)
	docker run -ti --link $(NAME):$(NAME) hiromasaono/curl
	
clean:
	-docker stop $(NAME)
	-docker rm $(NAME)
	
push:
	gcloud docker push $(TAG)

deploy: build push
	-kubectl delete svc $(NAME)
	-sed -e 's~<CONTAINER>~$(TAG)~g' deployment.yaml | kubectl apply -f -
	-kubectl create -f svc.yaml
