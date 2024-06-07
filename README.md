## Projeto: Rodando sua Aplicação Java no Kubernetes. Do Deploy ao Debug sem Medo!

Para criar um projeto de **Kubernetes** que permita rodar uma aplicação Java com a capacidade de fazer deploy e debug sem medo, você pode seguir os seguintes passos, organizados em módulos:

### DESCRIÇÃO DO PROJETO:

### Módulo 1: Preparação do Ambiente Local
- **Instalação do Minikube**: Minikube é uma ferramenta que permite rodar o Kubernetes localmente. Ele cria uma máquina virtual no seu computador e roda um cluster de Kubernetes de nó único dentro dessa VM.
- **Instalação do kubectl**: É a ferramenta de linha de comando para interagir com o cluster Kubernetes.

### Módulo 2: Criação da Aplicação Java
- **Desenvolvimento da Aplicação**: Crie uma aplicação Java simples, como um serviço web Spring Boot.
- **Dockerização**: Crie um Dockerfile para empacotar a aplicação Java em uma imagem Docker.

### Módulo 3: Configuração do Kubernetes
- **Deployment**: Escreva um arquivo YAML para definir o deployment da sua aplicação no Kubernetes.
- **Serviços**: Defina um serviço Kubernetes para expor sua aplicação para acesso externo.

### Módulo 4: Deploy da Aplicação
- **Deploy no Minikube**: Use o kubectl para fazer o deploy da sua aplicação no cluster Minikube.
- **Verificação**: Verifique se a aplicação está rodando corretamente acessando o serviço exposto.

### Módulo 5: Debugging
- **Configuração de Debug**: Configure sua aplicação e o ambiente Kubernetes para permitir o debugging.
- **Ferramentas de Debug**: Utilize ferramentas como o IntelliJ IDEA ou o Eclipse para se conectar à aplicação rodando no Kubernetes e realizar o debug.

### Módulo 6: Monitoramento e Logs
- **Monitoramento**: Configure o Prometheus e o Grafana para monitorar o desempenho da sua aplicação.
- **Logs**: Aprenda a visualizar e interpretar os logs da aplicação usando o kubectl ou outras ferramentas como o Kibana.

Este projeto tem como objetivo ensinar como configurar e gerenciar um ambiente Kubernetes local usando Minikube para rodar uma aplicação Java. Você aprenderá a empacotar sua aplicação em uma imagem Docker, fazer o deploy no Kubernetes, e configurar o ambiente para permitir o debugging em tempo real. Além disso, o projeto abordará práticas de monitoramento e análise de logs para manter a saúde da aplicação.

**Objetivos**:
- Entender os conceitos básicos do Kubernetes.
- Aprender a empacotar e rodar uma aplicação Java no Kubernetes.
- Desenvolver habilidades de debugging em um ambiente de orquestração de contêineres.
- Adquirir conhecimento em monitoramento e análise de logs.

**Resultado Esperado**:
Ao final do projeto, você terá uma aplicação Java rodando em um cluster Kubernetes local com a capacidade de fazer deploy e debug de forma eficiente e segura.

Aqui estão exemplos de código para cada módulo do projeto Kubernetes com uma aplicação Java:

### Módulo 1: Preparação do Ambiente Local
```bash
# Instalação do Minikube
minikube start

# Instalação do kubectl
curl -LO "https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

### Módulo 2: Criação da Aplicação Java
```Dockerfile
# Dockerfile
FROM openjdk:8-jdk-alpine
VOLUME /tmp
ADD target/spring-boot-app.jar app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

### Módulo 3: Configuração do Kubernetes
```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: java-app-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: java-app
  template:
    metadata:
      labels:
        app: java-app
    spec:
      containers:
      - name: java-app
        image: java-app:latest
        ports:
        - containerPort: 8080

# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: java-app-service
spec:
  type: NodePort
  selector:
    app: java-app
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
```

### Módulo 4: Deploy da Aplicação
```bash
# Deploy no Minikube
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

# Verificação
minikube service java-app-service
```

### Módulo 5: Debugging
```bash
# Configuração de Debug no IntelliJ IDEA
# 1. Adicione a configuração de Remote JVM Debug no IntelliJ.
# 2. Conecte-se ao pod do Kubernetes usando o kubectl port-forward.
kubectl port-forward <nome-do-pod> 5005:5005
```

### Módulo 6: Monitoramento e Logs
```bash
# Monitoramento com Prometheus
# Instale o Prometheus no seu cluster Kubernetes usando Helm
helm install stable/prometheus

# Logs
kubectl logs <nome-do-pod>
```

Esses são exemplos básicos para começar com cada módulo. Lembre-se de ajustar os comandos e códigos conforme necessário para o seu ambiente específico e a configuração da sua aplicação.
