FROM docker/whalesay:latest

RUN apt-get -y update && apt-get install -y fortunes

CMD /usr/games/fortune -a | cowsay

#An image is a filesystem and parameters to use at runtime. It doesn’t have state and never changes. 
#A container is a running instance of an image. 