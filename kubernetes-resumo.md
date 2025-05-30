
# Resumo Kubernetes - Recursos e Conceitos Essenciais

Este documento fornece um resumo prático e direto dos principais recursos do Kubernetes, com exemplos prontos para aplicar e testar. Ideal para iniciantes e como guia de referência rápida.

---

## 🧱 Pod
- Unidade mais básica do Kubernetes que executa um ou mais contêineres.
- Compartilham rede e sistema de arquivos.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: meu-pod
spec:
  containers:
    - name: app
      image: nginx
```

---

## 🔁 ReplicaSet
- Garante que o número desejado de réplicas de um Pod esteja rodando.
- Gerenciado automaticamente por um Deployment na maioria dos casos.

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: meu-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: minha-app
  template:
    metadata:
      labels:
        app: minha-app
    spec:
      containers:
        - name: app
          image: nginx
```

---

## 🚀 Deployment
- Gerencia ReplicaSets e atualizações declarativas de Pods.
- Suporta rollbacks, rollouts e scaling.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: meu-deploy
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          imagePullPolicy: IfNotPresent
```

---

## 🏷️ Label Selector
- Usado para vincular objetos (Pods, Services, etc) com base em labels.

```yaml
selector:
  matchLabels:
    app: minha-app
```

---

## 🔄 Rollout
- Permite atualizações e reversões de versões de Deployments.

```bash
kubectl rollout status deployment meu-deploy
kubectl rollout undo deployment meu-deploy
```

---

## 🌐 Services
Permitem acesso estável aos Pods.

### 🔸 ClusterIP (Padrão)
- Acesso interno apenas dentro do cluster.

```yaml
spec:
  type: ClusterIP
```

### 🔸 NodePort
- Expõe o serviço via IP de qualquer nó do cluster.

```yaml
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 3000
      nodePort: 30000
```

### 🔸 LoadBalancer
- Cria um IP externo (em provedores como GCP, AWS).

```yaml
spec:
  type: LoadBalancer
```

### 🔸 ExternalName
- Redireciona para um DNS externo.

```yaml
spec:
  type: ExternalName
  externalName: example.com
```

---

## 🔗 Endpoints
- Criados automaticamente ao associar um Service a Pods via selector.
- Exibe os IPs dos Pods selecionados.

```bash
kubectl get endpoints nome-do-servico
```

---

## 🌱 Variáveis de Ambiente (env)
- Passam dados ao container no momento da criação.

```yaml
env:
  - name: AMBIENTE
    value: "producao"
```

---

## 🛠️ ConfigMap & Secret

### 🔸 ConfigMap
- Armazena dados de configuração não sensíveis.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: config-app
  namespace: default
```

### 🔸 Secret
- Armazena dados sensíveis codificados em base64.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: credenciais
```

---

## 📦 imagePullPolicy
- Define quando a imagem do container deve ser puxada:
  - `Always`: sempre puxa do registry
  - `IfNotPresent`: usa local se existir
  - `Never`: nunca puxa

```yaml
imagePullPolicy: IfNotPresent
```

---

## 🔄 restartPolicy
- Define comportamento de reinício dos Pods:
  - `Always`: sempre reinicia (default para Deployments)
  - `OnFailure`: reinicia somente em falhas
  - `Never`: nunca reinicia

```yaml
restartPolicy: Always
```

---

## 🧭 Command & Args
- Substituem `ENTRYPOINT` e `CMD` da imagem.

```yaml
command: ["sleep"]
args: ["3600"]
```

---

## ❤️ Self-Healing

### 🔸 Liveness Probe
Verifica se o container ainda está funcionando. Se falhar, o container será reiniciado.

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 10
```

### 🔸 Readiness Probe
Verifica se o container está pronto para receber requisições. Se falhar, ele será removido do Service.

```yaml
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 3
  periodSeconds: 5
```

### 🔸 Startup Probe
Usado para containers que demoram a iniciar. Ignora liveness e readiness até passar.

```yaml
startupProbe:
  httpGet:
    path: /startup
    port: 8080
  failureThreshold: 30
  periodSeconds: 10
```

---

✅ **Dica Final:** Use `kubectl explain` para consultar recursos:

```bash
kubectl explain pod.spec.containers.livenessProbe
```

---

Esse documento é ideal para revisar os principais tópicos de Kubernetes com exemplos diretos e prontos para uso. Recomendado para estudantes, desenvolvedores e SREs iniciando com Kubernetes.
