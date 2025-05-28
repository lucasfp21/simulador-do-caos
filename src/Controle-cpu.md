## Como limitar um container para consumir uma quantidade de CPU previamente estabelecida 
### O comando docker stats é usado para mostrar o consumo de memoria e CPU dos containeres rodando
        docker stats

## Para limitar o tempo no ciclo de processamento e quota de processamento entre os nucleos usamos os paramentros:
### --cpu-period para limitar o tempo em microsegundos
### --cpu-quota para limitar a quota de processamento dos nucleos disponiveis
        docker container run -d -p 8080:3000 --cpu-period=100000 --cpu-quota=50000 lufertony/simulador-do-caos:v1
    periodo definido em 100000 ms e quota em 50000

## Para definir um cpu espeficio para rodar o container podemos usar o paramentro --cpuset-cpus
        docker container run -d -p 8080:3000 --cpu-period=100000 --cpu-quota=50000 --cpuset-cpus=7 lufertony/simulador-do-caos:v1

## Outra forma de usar o controle de cpu no docker e usando o paramtro --cpus
### Onde é possivel especificar --cpus=0.5 para usar 50% de uma cpu ou ainda podemos usar --cpus=1.3 para usarmos 100% de uma cpu e 30% de outra cpu, nesse caso precisamos especificar todas as cpus usadas no paramentro --cpuset-cpus=2-4
        docker container run -d -p 8080:3000 --cpus=1.7 --cpuset-cpus=2-4 lufertony/simulador-do-caos:v1