# Projeto 4 – Monitoramento e Observabilidade

## Objetivo

Monitorar uma aplicação Java rodando em Kubernetes, coletando métricas com Prometheus e visualizando-as no Grafana.

---

## Stack Utilizada

* Kubernetes (Minikube ou Kind)
* Prometheus
* Grafana
* NGINX Exporter
* Spring Boot + Actuator + Micrometer

---

## Estrutura do Projeto

A estrutura real do projeto está organizada da seguinte forma:

```
.
├── README.md
├── dashboards
│   └── java-app-observability.json
├── grafana
│   ├── grafana-deployment.yaml
│   └── grafana-service.yaml
└── prometheus
    ├── namespace.yaml
    ├── nginx-config.yaml
    ├── nginx-deployment.yaml
    ├── nginx-exporter-service.yaml
    ├── nginx-exporter.yaml
    ├── nginx-service.yaml
    ├── prometheus-config.yaml
    ├── prometheus-deployment.yaml
    └── prometheus-service.yaml
```

---

## Métricas Coletadas

As métricas são expostas pela aplicação Java através do endpoint:

```
/actuator/prometheus
```

Principais métricas utilizadas:

* JVM (memória, GC, threads)
* HTTP Requests
* Latência das requisições
* Status HTTP (200, 400, 500)

---

## Consultas PromQL Utilizadas

### Taxa de requisições por status HTTP

```promql
sum by (status) (
  rate(http_server_requests_seconds_count[1m])
)
```

### Uso de memória JVM

```promql
jvm_memory_used_bytes
```

### Tempo médio de resposta

```promql
rate(http_server_requests_seconds_sum[1m])
/
rate(http_server_requests_seconds_count[1m])
```

---

## Grafana

### Dashboard

O dashboard foi criado manualmente e exportado para o arquivo:

```
dashboards/java-app-observability.json
```

### Importação do Dashboard

1. Acesse o Grafana
2. Vá em **Dashboards → Import**
3. Faça upload do arquivo JSON
4. Selecione o datasource Prometheus
5. Clique em **Import**

---

## Como Executar o Projeto

### 1. Criar namespace

```bash
kubectl apply -f prometheus/namespace.yaml
```

### 2. Subir Prometheus

```bash
kubectl apply -f prometheus/prometheus-config.yaml
kubectl apply -f prometheus/prometheus-deployment.yaml
kubectl apply -f prometheus/prometheus-service.yaml
```

### 3. Subir NGINX Exporter

```bash
kubectl apply -f prometheus/nginx-config.yaml
kubectl apply -f prometheus/nginx-deployment.yaml
kubectl apply -f prometheus/nginx-service.yaml
kubectl apply -f prometheus/nginx-exporter.yaml
kubectl apply -f prometheus/nginx-exporter-service.yaml
```

### 4. Subir Grafana

```bash
kubectl apply -f grafana/grafana-deployment.yaml
kubectl apply -f grafana/grafana-service.yaml
```

### 5. Acessar Grafana

```bash
kubectl port-forward svc/grafana 3000:3000
```

Acesse em: [http://localhost:3000](http://localhost:3000)

---

## Resultado Final

* Aplicação Java com métricas expostas
* Prometheus coletando métricas
* Grafana exibindo dashboards funcionais
* Monitoramento por status HTTP e performance

---

## Conclusão

Este projeto demonstra na prática conceitos fundamentais de observabilidade em ambientes Kubernetes, incluindo coleta de métricas, visualização e análise de performance, seguindo boas práticas de DevOps.

## Conclusão

Este projeto demonstra, de forma prática e funcional, a implementação de **monitoramento e observabilidade** para uma aplicação Java executando em **Kubernetes**, utilizando **Prometheus** e **Grafana**.

Ao longo do projeto, foi possível:

* Expor métricas da aplicação via **Spring Boot Actuator** no endpoint `/actuator/prometheus`;
* Configurar o **Prometheus** para coletar métricas da aplicação e de componentes de infraestrutura;
* Visualizar métricas reais de JVM, requisições HTTP e status de respostas;
* Criar e exportar um **dashboard funcional no Grafana**, permitindo análise clara do comportamento da aplicação;
* Organizar a infraestrutura de forma modular e reproduzível, seguindo boas práticas de projetos DevOps.

O ambiente simula um cenário real de produção, evidenciando domínio dos conceitos de **observabilidade**, **monitoramento**, **PromQL**, **Kubernetes** e **boas práticas de infraestrutura**.

Este projeto consolida conhecimentos essenciais para ambientes cloud-native e serve como um **artefato de portfólio**, demonstrando capacidade de diagnosticar, analisar e acompanhar a saúde de aplicações em produção.

