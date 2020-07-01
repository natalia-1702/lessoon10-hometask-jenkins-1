2 таски для jenkins
1 задача - собирается докеробраз на удаленной ВМ и пушится в докерхаб
2 задача - пулл-ится докеробраз с докерхаб и создается контейнер с приложением, которое доступно по адресу: http://84.201.148.188:8080/hello-1.0/


----------------

Если же используем нексус вместо докехраб, то:


1 хост  джоб

#parametr version=1.1
apt update && apt install docker.io

cat << EOF | tee /etc/docker/daemon.json
{
    "insecure-registries" : ["84.201.150.2:8123]
}
EOF


rm -rf Docker-maven-helloworld-lesson6
git clone https://github.com/natalya-limareva/Docker-maven-helloworld-lesson6.git
docker build -t 84.201.150.2:8123/javaapp:$version ./Docker-maven-helloworld-lesson6/
docker images | grep javaapp
docker login 84.201.150.2:8123  -u 17021993 -p 17021993Nv
docker push 84.201.150.2:8123/javaapp:$version


2 хост  джоб

#parametr version=1.1
apt update && apt install docker.io

cat << EOF | tee /etc/docker/daemon.json
{
    "insecure-registries" : ["84.201.150.2:8123]
}
EOF


docker login 84.201.150.2:8123  -u 17021993 -p 17021993Nv
docker pull 84.201.150.2:8123/javaapp:$version
docker run -d -p 8080:8080 84.201.150.2:8123/javaapp:$version
