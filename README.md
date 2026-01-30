# Projeto 04 – Monitoramento e Observabilidade com Prometheus e Grafana

Este projeto tem como objetivo demonstrar, **de forma prática e organizada**, a implementação de monitoramento e observabilidade em um ambiente Kubernetes utilizando **Prometheus** e **Grafana**.

O foco é simular um cenário real de produção, comum ao dia a dia de um **Analista DevOps Júnior**, monitorando serviços, exporters e uma aplicação Java.

---

## 🎯 Objetivos do Projeto

* Implantar Prometheus no Kubernetes
* Configurar coleta de métricas (scrape)
* Monitorar serviços e exporters
* Visualizar métricas em dashboards no Grafana
* Validar o status dos targets (UP / DOWN)
* Criar dashboards funcionais e reutilizáveis

---

## 🛠️ Stack Utilizada

* Kubernetes (Minikube ou Kind)
* Prometheus
* Grafana
* NGINX
* NGINX Prometheus Exporter
* Aplicação Java (expondo métricas via `/actuator/prometheus`)

---

## 📁 Estrutura do Projeto

```
project-04-observability/
├── README.md
├── dashboards/
│   ├── java-app-observability.json
│   └── observability-dashboard.json
├── grafana/
│   ├── grafana-deployment.yaml
│   └── grafana-service.yaml
└── prometheus/
    ├── namespace.yaml
    ├── nginx-config.yaml
    ├── nginx-deployment.yaml
    ├── nginx-exporter.yaml
    ├── nginx-exporter-service.yaml
    ├── nginx-service.yaml
    ├── prometheus-config.yaml
    ├── prometheus-deployment.yaml
    └── prometheus-service.yaml
```

---

## 🚀 Como Executar o Projeto

### 1️⃣ Criar o namespace

```bash
kubectl apply -f prometheus/namespace.yaml
```

---

### 2️⃣ Subir o NGINX e o Exporter

```bash
kubectl apply -f prometheus/nginx-config.yaml
kubectl apply -f prometheus/nginx-deployment.yaml
kubectl apply -f prometheus/nginx-service.yaml
kubectl apply -f prometheus/nginx-exporter.yaml
kubectl apply -f prometheus/nginx-exporter-service.yaml
```

Valide:

```bash
kubectl get pods -n observability
```

---

### 3️⃣ Subir o Prometheus

```bash
kubectl apply -f prometheus/prometheus-config.yaml
kubectl apply -f prometheus/prometheus-deployment.yaml
kubectl apply -f prometheus/prometheus-service.yaml
```

Verifique os targets no Prometheus:

```bash
kubectl port-forward svc/prometheus -n observability 9090:9090
```

Acesse:

```
http://localhost:9090/targets
```

---

### 4️⃣ Subir o Grafana

```bash
kubectl apply -f grafana/grafana-deployment.yaml
kubectl apply -f grafana/grafana-service.yaml
```

Acesse o Grafana:

```bash
kubectl port-forward svc/grafana -n observability 3000:3000
```

URL:

```
http://localhost:3000
```

Credenciais padrão:

* **Usuário:** admin
* **Senha:** admin

---

## 📊 Configuração do Grafana

### Adicionar o Prometheus como Data Source

* Acesse: **Settings → Data Sources → Add data source**
* Escolha: **Prometheus**
* URL:

```
http://prometheus.observability.svc.cluster.local:9090
```

* Clique em **Save & Test**

---

## 📈 Dashboards

Os dashboards prontos estão disponíveis na pasta `dashboards/`.

### Importar dashboards

1. Acesse **Dashboards → Import**
2. Cole o conteúdo do arquivo JSON ou faça upload
3. Selecione o Prometheus como Data Source

Dashboards incluídos:

* Observability Dashboard (Prometheus + NGINX)
* Java Application Observability

---

## ✅ Métricas Monitoradas

* Status dos targets (UP / DOWN)
* Séries ativas do Prometheus
* Uso de CPU e Memória do Prometheus
* Conexões ativas do NGINX
* Métricas da aplicação Java

---

## 📌 Aprendizados

* Configuração real de scrape no Prometheus
* Troubleshooting de targets DOWN
* Integração Prometheus + Grafana
* Importação e criação de dashboards
* Observabilidade aplicada em Kubernetes

---

## 👤 Autor

**Daniel Viana**
📧 Email: [daniel-viana2127@yahoo.com](mailto:daniel-viana2127@yahoo.com)
🔗 GitHub: [https://github.com/danielviana2127](https://github.com/danielviana2127)