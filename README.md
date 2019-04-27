# docker
Docker
- Possuí nomes únicos
- Não faz sentido ter um container isolado. Ele precisa ter um isolamento controlado
- Comunicação entre multiplos containers

# Iniciando - Hello World
docker container run hello-world

# Comando "run"
ele é a concatenação de 4 comandos:
 1º Docker image pull
 2º Docker container create
 3º Docker container start
 4º Docker container exec

Sempre cria uma novo container quando ele é executado

Outros exemplos de uso:

docker container run -it debian bash <- este comando irá entrar dentro do terminal do container
 - touch curso-docker.txt
 - ls curso-docker.txt
 - exit
 Se executar novamente:
 - docker container run -it debian bash 
 - ls curso-docker.txt
 O arquivo não existirá

Resumindo: Sempre que executar o comando run, ele cria novos containerus
 
docker container run --help <- ajuda
docker container run --name  mydeb -it debian bash <- o nome é unico, se executado novamente ele dará um erro pois o container já existe
docker container run -p 8080:80 nginx <- iniciando um container e expondo uma porta. A primeira porta (8080) é a porta exposta para fora do container, e a segunda porta (80) é a porta interna do container.

# Comando "start"
docker container start -ai mydeb <- a letra "a" é para acesso ao terminal e o "i" é em modo interativo ~ equivalente ao comando -t -image
 - touch curso-docker.txt <- se você executar este comando e depois sair do container, quando você voltar nele (mydeb), ele vai iniciar o container antido novamente

#Comandos Uteis para o Container

## ps
docker container ps <- apresenta os containers ativos no momento
docker container ps -a <- apresenta todos os containers, independente dd estado
## ls
docker container ls <- lista os containers ativos
docker container ls -a <- lista todos os containers
## stop
docker container stop nomecontainer ou o CONTAINER-ID
## start 
docker container start nomecontainer
## restart
docker container restart nomecontainer
## list
docker container list 
## exit
docker conatiner exec nomecontainer uname -or
 
# Comandos Úteis para a Imagem
## ls
docker image ls <- lista todas as imagens
## rm
docker image rm idimagem

# Comandos úteis para o volume 
docker volume ls 
 
# Testes com o NGINX
docker container run -p 8080:80 nginx
powershel: Invoke-RestMethod -Uri http://localhost:8080

# Testes com o Debian
docker container run debian bash --version

# Mapeamento de Volumes
1º Crie uma pasta no computador
2º docker container run -p 8080:80 -v C:/dev/docker/containers/ex-volume/html:/usr/share/nginx/html nginx <- -v (volume) de um lado é o caminho local que você deseja mapear, e do outro lado dos dois pontos, é o caminho do container
3º Criar um arquivo index.html
4º Ao acessar o browser, você vera o html no servidor

# Executando um container em backgroud
docker container run -d --name ex-daemon-basic -p 8080:80 -v C:/dev/docker/containers/ex-volume/html:/usr/share/nginx/html nginx
docker container stop ex-daemon-basic <- para o container que está rodando em backgroud
docker container start ex-daemon-basic <- iniciar novamente o container
docker container restart ex-daemon-basic

# Acesando os logs de um Container
docker container start ex-daemon-basic
docker container logs ex-daemon-basic

# Listando as configurações do container com o docker Inspect 
docker container inspect ex-daemon-basic

