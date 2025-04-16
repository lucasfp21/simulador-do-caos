### Para limitar a memoria do container via comando:
        docker container run -d -p 8080:3000 --memory=100m --memory-swap=200m lufertony/simulador-do-caos:v1

* --memory=<valor/m>
* --memory-swap<valor/m>
    * nesse caso do swap sempre somamos o valor do --memory + quantidade desejada no swap
    * na cloud nao tem swap
    * se bater o limite de memoria o container morre com status out of memory

### Exemplio controle de memoria com compose:

services:
  web:
    image: lufertony/simulador-do-caos:v2
    ports:
      - 8080:3000
    restart: always
    cpuset: "4"
    memswap_limit: 512M
    deploy:
      resources:
        limits:
          cpus: "0.5"
          memory: 512M