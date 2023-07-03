```
docker run -d -p 8080:80 --name webserver nginx
docker ps
docker images
docker container exec -it  webserver bash
docker stop webserver
docker rm webserver
docker rmi nginx
docker build -t hello-world:v1 .
docker create volume
docker vulume ls
docker compose build
docker compose start
docker compose stop
docker compose up -d
docker compose logs -f web-fe
docker compose down
docker tag my_image k8sacademy/my_image:latest
docker push k8sacademy/my_image:latest
docker pull k8sacademy/my_image:latest
```