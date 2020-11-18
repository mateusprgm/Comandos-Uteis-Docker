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

`$ docker run -it jboss/wildfly /bin/bash`

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

Para entrar dentro de um container em execução

`$ docker exec -it id-container bash`

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

## Docker-Compose

### O que é o docker-compose

O docker-compose é uma ferramenta cujo o objetivo é facilitar o processo de executar containers docker de forma declarativa. Cada container é executado como um serviço.

O arquivo utilizado para que o docker-compose seja executado com sucesso chama-se por padrão docker-compose.yaml.

Exemplo:

	
	version:'3'
	services:
	  hello-world:
		image hello-world
		volumes:
		 - ./hello-world:/usr/share/hello-world/html/
		ports:
		 - 8080:80
		jboss/wildfly:
		  image: jboss/wildfly
		  expose:
		   - 6379


Se você verificar o exemplo acima, perceberá que teremos dois serviços a serem executados.

1. O primeiro chama-se hello-world. Ele utilizará a imagem do hello-world como base e fará um compartilhamento de volume, ou seja, a pasta local do computador será compartilhada com o container. Nesse caso, tudo que existir na pasta hello-world do computador, será automaticamente replicado no endereço : /usr/share/hello-world/html/ do container. Também a porta 8080 será redirecionada para o computador: localhost:8080 automaticamente o docker fará o redirecionamento da requisição para a porta 80 do container.

2. O segundo serviço chama-se jboss/wildfly e nesse caso é extremamente simples.
Ele utiliza o jboss/wildfly como imagem base e expõe a porta 6379 do container. Isso significa que o container do hello-world poderá se comunicar na rede local criada pelo docker utilizando a porta 6379.

### Comandos úteis para o docker-compospe

Para iniciar os serviços declarados do docker-compose.yml, basta executar:
`$ docker-compose up`

Ao executar esse comando, os serviços serão inicializados, porém, você perceberá que seu terminal ficará bloqueado, uma vez que o processo está sendo executado. Para executar de forma desatachada, basta informar o parâmetro "-d" no final da instrução.

Para encerrar os serviços, basta executar: 

`$ docker-compose down`

Caso queira ver de forma mais "organizada" somente os containers dos serviços sendo executados, basta rodar:

`$ docker-compose ps`

Fonte: Full Cycle - Aulão Docker - Comece com Docker do Zero