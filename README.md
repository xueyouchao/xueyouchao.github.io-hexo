Pre-install git & node.js



git clone https://github.com/xueyouchao/xueyouchao.github.io-hexo.git

enter blog folder and run:  
npm install
(ensure the node version is 4.2.1 on linux64)

update sub module:  
git submodule init  
git submodule update

build image from https://github.com/xueyouchao/myzsh:
rename DockerfileForResume to Dockerfile
docker build -t . myzshcontainer
docker run -ti --rm --name myzshcontainer -v C:\xueyouchao.github.io-hexo:/home/zsher/workspace myzsh:latest