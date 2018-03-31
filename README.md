# Docker


## Comandos Básicos

*1. testando instalação* <br/>
`docker container run hello-word` <br/>
>   *run > 4 em 1* <br/>
>   docker image pull, <br/>
>   docker container create, <br/>
>   docker container start, <br/> 
>   docker container exec <br/>

*2. executando programa do container* <br/>
`docker container run debian bash --version` <br/>

*3. Operações Docker* <br/>
>   *Listar Containers (ls ou ps)* `docker container ls` <br/>
>   *Listar Imagens* `docker image ls` <br/>
>   *Listar Volume* `docker volume ls` <br/>
>   *Remover Containers* `docker container rm e3e9d0b51c74` <br/>
>   *Remover Imagens* `docker image rm cae870735e91` <br/>
>   *Remover Volume* `docker volume rm not-found` <br/>

*lista todos os containers independente do status* <br/>
`docker container ps -a` <br/>
ou <br/>
`docker container ls -a` <br/>

*4. remove o container da listagem* <br/>
`docker container run --rm debian bash --version` <br/>

*5. (-i) acesso interativo, (-t) acesso ao terminal* <br/>
`docker container run --it debian bash` <br/>
>   touch curso-docker.txt <br/>
>   ls curso-docker.txt <br/>
>   exit <br/>

*6. obter comandos válidos* <br/>
`docker container run --help` <br/>

*7. (--name) nomear um container* <br/>
`docker container run --name mydeb -it debian bash` <br/>

*8. reutilizar um container* <br/>
`docker container run --name mydeb -it debian bash` <br/>

*9. (-a) anexar, (-i) acesso interativo* <br/>
`docker container start -ai mydeby` <br/>

*10. Obtendo Ajuda*
>   *Help container* `docker container --help` <br/>
>   *Help image* `docker image --help` <br/>
>   *Help volume* `docker volume --help` <br/>


## Isolamento Controlado

*1. (-p) porta, (8080) porta exposta para acesso, (80) porta interna do server nginx* <br/>
`docker container run -p 8080:80 nginx` <br/>

*2. (-v) mapear diretório local, (%cd%) diretório atual* <br/>
`docker container run -p 8080:80 -v %cd%/html:/usr/share/nginx/html nginx` <br/>
>   %cd%/html/index.html


## Docker em Background

*1. (-d) modo daemon* <br/>
`docker container run -d --name ex-basic -p 8080:80 -v %cd%/html:/usr/share/nginx/html nginx` <br/>

*2. stop container modo daemon (por nome)* <br/>
`docker container stop ex-basic` <br/>

*3. stop container modo daemon (por ID)* <br/>
`docker container stop e3e9d0b51c74` <br/>

*4. start container modo daemon* <br/>
`docker container start ex-basic` <br/>

*5. restart container modo daemon* <br/>
`docker container restart ex-basic` <br/>


## Inspecionar um Container

*1. logs do container* <br/>
`docker container logs ex-basic` <br/>

*2. inspect do container* <br/>
`docker container inspect ex-basic` <br/>

*3. (exec) executa um commando em um container ativo* <br/>
`docker container exec ex-basic uname -or` <br/>


## Trabalhando com Imagens

*1. pull image* <br/>
`docker image pull redis:latest` <br/>

*2. inspect image* <br/>
`docker image inspect redis:latest` <br/>

*3. (-t) tag image* <br/>
`docker image tag redis:latest cod3r-redis` <br/>

*4. excluindo duas imagens* <br/>
`docker image rm redis:latest cod3r-redis` <br/>

*5. Construindo Imagens* <br/>
>   *build image (sem args)* `docker image build -t ex-build-first %cd%` <br/>
>   *executando (sem args)* `docker container run -p 80:80 ex-build-first` <br/>

>   *build image (com args)* `docker image build --build-arg S3_BUCKET=myapp -t ex-build-arg %cd%` <br/>
>   *executando (com args)* `docker container run ex-build-arg bash -c "echo $S3_BUCKET"` <br/>

*6. Inspect properties* <br/>
`docker image inspect --format="{{index .Config.Labels \"maintainer\"}}" ex-build-arg` <br/>


## Docker Hub x Docker Registry

*Docker Registry* <br/>
>   Serviço server-side para registro e obtenção de imagens <br/>
>   Disponibiliza API para enviar e obter imagens apartir do serviço <br/>
>   Pode ser instalado um Registry privado em qualquer ambiente de interesse <br/>
>   Por exemplo: na empresa, na residência, para a criação e execução das imagens <br/>

*Docker Hub* <br/>
>   SAAS - Softwarer As A Service <br/>
>   Na nuvem, disponibilizado pelo Docker.inc  <br/>
>   Dentro do Docker Hub existe um Registry <br/>


## Imagens no Docker Hub

*1. Login Docker e Docker Hub* <br/>
`docker login --username=jpralves` <br/>

*2. Create image for dkhub* <br/>
`docker image tag ex-simple-build jpralves/simple-build:1.0` <br/>

*3. Push image for dkhub* <br/>
`docker image push jpralves/simple-build:1.0` <br/>


## Redes em Docker

*1. Listando networks existentes* <br/>
`docker network ls` <br/>

*2. Inspecionar determinada rede* <br/>
`docker network inspect bridge` <br/>

*3. Rede tipo none (--net none)* <br/>
`docker container run -d --net none debian` <br/>
`docker container run --rm alpine ash -c "ifconfig"` <br/>
`docker container run --rm --net none alpine ash -c "ifconfig"` <br/>

*4. Redes tipo bridge (--net bridge)* <br/>
>   *Teste de rede entre containers (mesma rede)* <br/>
>   *Container1* <br/>
>   `docker container run -d --name container1 alpine sleep 1000` <br/>
>   `docker container exec -it container1 ifconfig` <br/>
>   *Container2* <br/>
>   `docker container run -d --name container2 alpine sleep 1000` <br/>
>   `docker container exec -it container2 ifconfig` <br/>
>   *Container1 ping Container2* <br/>
>   `docker container exec -it container1 ping 172.17.0.3` <br/>
>   *Container1 ping externo* <br/>
>   `docker container exec -it container1 ping www.google.com` <br/>

*5. Criando nava rede* <br/>
`docker network create --driver bridge rede_nova` <br/>

*6. Utilizando rede nova* <br/>
`docker container run -d --name container3 --net rede_nova alpine sleep 1000` <br/>
`docker container exec -it container3 ifconfig` <br/>

*7. Ping em outra rede (não pinga)* <br/>
`docker container exec -it container3 ping 172.17.0.2` <br/>

*8. Conectando nova_rede a rede bridge* <br/>
`docker network connect bridge container3` <br/>
`docker container exec -it container3 ifconfig` <br/>
`docker container exec -it container3 ping 172.17.0.2` <br/>

*9. Desconectando container3 da rede bridge* <br/>
`docker network disconnect bridge container3` <br/>

*10. Rede tipo host (--net host)* <br/>
`docker container run -d --name container4 --net host alpine sleep 1000` <br/>
`docker container exec -it container4 ifconfig` <br/>


## Docker Compose e Micro Serviços

*1. Startando serviços configurados* <br/>
`docker-compose up` <br/>


## Erros Comuns

*Erro: `Error unauthorized: incorrect username or password`* <br/>
*Procedimento: `docker logout`* <br/>

*Erro: `... unauthorized: incorrect username or password`* <br/>
*Procedimento: `docker login`* <br/>


## Observações Importantes


## Créditos

jpralves all rights copyright@
