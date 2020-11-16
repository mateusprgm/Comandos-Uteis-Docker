# Comandos-Uteis-Docker

## Oque é Docker

### Por que utilizar o Docker

É um softwqare que utiliza um mecanismo de execução de containers através de processos.

### Quais os pontos chave que o Docker utiliza para rodar de forma estável?

Porque diferentemente de máquinas virtuais, o Docker consegue de forma extremamente rápida e leve executar seus containers sem a necessidade de instalação completa de um sistema operacional. O docker "empresta" todos os recursos, incluindo o Kernel para que seus containers possam ser utilizar.

### Pontos chave para funcionar

1. Namespaces - Isolamento de processos
2. Cgroups - Controla os recursos do sistema operacional
3. Sistema de arquivos - OFS (Overlay File System)

## Docker foi feito para LINUX

O Docker foi feito para rodar no Linux. Logo, o melhor comportamento, você pode esperar rodando no Linux!

Porém, você pode rodar Docker no Windows ou no Mac, todavia, você pode enfrentar alguns desafios.

Windows HOME não possui um recurso padrão, o HyverV, logo, normalmente é utilizado a famosa Docker ToolBox.

Recomendação: Utilize o Windows Professional.

## Comandos Básicos

### Containers

Executar um container:

`$ docker run -it ubuntu /bin/bash`

Verificar containers em utilização:
	
`$ docker ps`
	
Verificar todos os containers:
	
`$ docker ps -a`

Compartilhamento / Exposição de portas

`$ docker run -p 8080:80 jboss/wildfly`

Executar um container em segundo plano (modo datached)

`$ docker run -d -p 8080:80 jboss/wildfly`

Remover container após o stop

`$ docker run --rm -d -p 8080:80 jboss/wildfly`

Remover todos os containers (-f para forçar até os que estão em execução a ser removido)

`$ docker rm $(docker ps -a -q) -f`

### Imagens

O que são imagens?

Imagens a grosso modo são uma espécie de template de container que será gerado. A imagem é imutável. Toda imagem possui uma camada de gravação de arquivos.

Remover uma imagem

`$ docker rmi <nome da imagem>`

Remover todas as imagens

`$ docker rmi $(docker images -q)`

### Dockerfile

É um arquivo declarativo que tem o objetivo construir uma imagem através de outra imagem.

O Dockerfile possui toda a estrutura e comandos necessários para que ações sejam executadas no processo de "build", ou seja, no processo de construição de uma imagem.

### Gerando build de uma imagem

Após a criação do arquivo Dockerfile, você poderá criar sua própria image utilizando o seguinte comando:

`$ docker build -t <seu-user>/nome-da-image>:<versao-da-image>`


### Publicando image no Docker Hub

O Dockerhub é um repositório aonde você pode disponibilizar suas imagens, de forma pública ou privada. Para que a publicação seja possível, você primeiramente terá que realizar o login em sua conta digitando 

`$ docker login`

Realizado o login basta realizar o push de sua image:

`$ docker push <nome-da-image>`

Fonte: Full Cycle - Aulão Docker - Comece com Docker do Zero