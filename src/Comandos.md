Se o retorno do STATUS for diferente de 0 entao o conteiner terminou com erro
    CONTAINER ID   IMAGE                            COMMAND                  CREATED          STATUS                     PORTS     NAMES
    6b65f89a8b97   lufertony/simulador-do-caos:v1   "docker-entrypoint.s…"   12 seconds ago   Exited (1) 6 seconds ago             silly_hofstadter   
    06fd879e063f   lufertony/simulador-do-caos:v1   "docker-entrypoint.s…"   3 minutes ago    Exited (0) 3 minutes ago             mystifying_gauss

docker restart on-failure
    usado somente se o container for encerrado com erro (status != 0)
        docker container run -d -p 8080:3000 --restart=on-failure lufertony/simulador-do-caos:v1
        docker container run <-d ou -it> <-p porta:porta> --restart=<tipo de restart> <imagem>:<versao>

    para determinar a quantidade de restarts, basta adionar :<quantidade> apos o parametro
        docker container run -d -p 8080:300 --restart=on-failure:3 lufertony/simulador-do-caos:v1

    o restart on-failure vai restartar o container mesmo em caso do docker em si parar e voltar, o unnico caso onde o restart nao acontece é quando o container e encerrado com sucesso.

PS. podemos usar o watch 'docker ps -a' para observar os restarts no terminal

restart unless-stopped:
    só vai parar de restartar o container caso em caso de parada manual com o comando docker container stop <id>
        docker container run -d -p 8080:3000 --restart=unless-stopped lufertony/simulador-do-caos:v1

    para parar esse container somente usando o stop:
        docker container stop <id>
        docker container start <id>

restart always
    restarta o container em todos os cenarios, em caso de pausa com erro, com sucesso ou queda do docker
         docker container run -d -p 8080:3000 --restart=always lufertony/simulador-do-caos:v1

    o docker start/stop funciona nesse caso tambem

