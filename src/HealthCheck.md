# Healthcheck:

## um verificação por comando, basicamente voce um instrução para ele executar e toma alguma acao dependendo do resultado 

### Healthcheck via linha de comando
        docker run -d -p 8080:3000 --health-cmd "curl -f http://localhost:3000/health" health-timeout 5s --health-retries 3 --health-interval 10s --health-start-period 30s lufertony/simulador-do-caos
           
            o health-cmd é o comando que vai ser testado
            o health-timeout é o tempo de cada execução
            o health-retries é a quantidade de tentativas de rodar o comando health-cmd
            o health-interval ó o tempo entre uma execução e outra
            o health-start-period é a quantidade de tempo pra iniciar o health-cmd

### Healthcheck no docker compose:
    healthcheck:
      test: ["CMD", "curl","-f","http://localhost:3000/healthcheck"]
      interval: 5s
      timeout: 10s
      retries: 2
      start_period: 30s
      disable: true

### Healthcheck no DOCKERFILE:
    HEALTHCHECK --interval=10s --timeout=5s --start-period=30s --retries=2 CMD [ "curl", "-f", "http:localhost:3000/health" ]

    depois tem que fazer o bild da imagem:
        docker build -t <nome da imagem>:<tag> -f <caminho/dockerfile> <path/de/contexto>

