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
	-kubectl delete rc $(NAME)-rc
	-kubectl delete svc $(NAME)
	-sed -e 's~<CONTAINER>~$(TAG)~g' rc.yaml | kubectl create -f -
	-kubectl create -f svc.yaml
