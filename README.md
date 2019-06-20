# Micorservices

Если надо, логинимся в gcloud

gcloud init 
gcloud auth application-default login

Создаем docker-machine

export GOOGLE_PROJECT=_ваш-проект_ 
docker-machine create --driver google \
 --google-machine-image https://www.googleapis.com/compute/v1/projects/ubuntu-os-cloud/global/images/family/ubuntu-1604-lts \
 --google-machine-type n1-standard-1 \
 --google-zone europe-west1-b \
 docker-host
docker-machine ls
eval $(docker-machine env docker-host) 

Собираем приложение в виде микросервисов

docker build -t <your-dockerhub-login>/post:1.0 ./post-py
docker build -t <your-dockerhub-login>/comment:1.0 ./comment
docker build -t <your-dockerhub-login>/ui:1.0 ./ui

Запускаем

docker network create reddit
docker volume create reddit_db
docker run -d --network=reddit --network-alias=post_db \
--network-alias=comment_db -v reddit_db:/data/db mongo:latest
docker run -d --network=reddit \
--network-alias=post <your-dockerhub-login>/post:1.0
docker run -d --network=reddit \
--network-alias=comment <your-dockerhub-login>/comment:1.0
docker run -d --network=reddit \
-p 9292:9292 <your-dockerhub-login>/ui:1.0

Убиваем контейнеры

docker kill $(docker ps -q)

docker.io/andruccho
