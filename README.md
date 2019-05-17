# docker
Docker
- Possuí nomes únicos
- Não faz sentido ter um container isolado. Ele precisa ter um isolamento controlado
- Comunicação entre multiplos containers

## Diferença entre image e container
- A imagem é quivalente a classe e o container seria um objeto. Com a classe (imagem) é possível instanciar vários containers
- Uma imagem é um modelo de sistema de arquivos somente leitura formado em camadas.
- O Container é um processo segredo e isolado, a partir deste processo pode ser criado muitos sub processos e este tem acesso a um sistema de arquivos criado a partir da imagem.
- O conatiner é o processo e a imagem é o modelo de sistema arquivos para a criação de containers

## Image
- Possuí um identificador único, que é um SHA 256. Mas não é comum utilizado, é melhor ter identificador e tags para acessa-lás
- São organizadas em registrys (repositórios) como o Docker Hub
- No docker hub só tem imagens, que são utilizadas para inicar os containers
- Para utilizar uma imagem com a tag latest, é preciso utilziar o comando: docker image pull redis:latest (APENAS DOWNLOAD)
- Para inspencionar uma imagem docker é preciso utilizar o comando:  docker image inspect redis:latest
- Para inserir tags nas imagens é preciso utilizar o comando: docker image tag redis:latest cod3r-redis e depois docker image ls para ver o resultado
- Para remover uma imagem é preciso executar o comand: docker image rm redis:latest cod3r-redis e depois docker image ls para ver o resultado
- As hashs representar as versões das imagens, é possível ter várias tags apontando para uma mesma imagem
- Não é bom utilizar as ultímas versões das imagens, pode quebrar os projetos

### comandos para Image
- docker image ls <- listar as imagens
- docker image pull <- responsável por baixar as imagens do docker hub (as vezes acontece de forma implicita, como por exemplo, quando é utilizando do comando docker run)
- docker image rm <- remover as imagens, aceita o hash ou o nome da imagem
- docker image inspect <- utilizar para inspencionar uma imagem
- docker image tag <- o 1º parametro é a imagem fonte que recebera a tag e depois a tag
- docker image build <- constroi uma imagem atraves de um arquivo
 
### imagem build command
- docker image build -t ex-simple-build C:\dev\docker\primeiro-build <- o ponto no final é a pasta atual aonde está o arquivo Dockerfile
- docker container run -p 80:80 ex-simple-build

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

## Utilizando SQL SERVER

Comando para realizar o download da Imagem do SQL SERVER 2017: <br />
`docker pull mcr.microsoft.com/mssql/server:2017-latest` 

Inicio o Banco de Dados: <br />
`docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=789@QaZ' -p 1433:1433 --name sql1 -d mcr.microsoft.com/mssql/server:2017-latest` 

Alterar a Senha do Banco de Dados: <br />
`docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourStrong!Passw0rd>' -Q 'ALTER LOGIN SA WITH PASSWORD="<YourNewStrong!Passw0rd>"'` 

### Conectando ao SQL SERVER

Inicia o Bash: <br />
`docker exec -it sql1 "bash"` 

Abre a conexão com o Banco de Dados: <br />
`/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourNewStrong!Passw0rd>'` 

### Conectando de fora de um container

Com o ip da maquina e sqlcmd instalado, executar o comando abaixo
`sqlcmd -S <ip_address>,1433 -U SA -P '789@QaZ'`

