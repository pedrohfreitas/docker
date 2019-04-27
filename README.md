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

# Comando "start"
docker container start -ai mydeb <- a letra "a" é para acesso ao terminal e o "i" é em modo interativo ~ equivalente ao comando -t -image
 - touch curso-docker.txt <- se você executar este comando e depois sair do container, quando você voltar nele (mydeb), ele vai iniciar o container antido novamente

#Comandos Uteis

## ps
docker container ps <- apresenta os containers ativos no momento
docker container ps -a <- apresenta todos os containers, independente dd estado
## ls
docker container ls <- lista os containers ativos
docker container ls -a <- lista todos os containers
 
# Testes
docker container run debian bash --version

