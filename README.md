# 📊 Project 04 – Observability (Prometheus + Grafana)

## 🎯 Objetivo

Este projeto demonstra, de forma **prática e aplicada**, a implementação de **monitoramento e observabilidade** em um ambiente Kubernetes utilizando **Prometheus** e **Grafana**.

O foco é educacional e de portfólio DevOps, mostrando domínio de métricas, dashboards e validação de dados em cluster.

---

## 🧱 Stack utilizada

* Kubernetes
* Prometheus
* Grafana
* kubectl

---

## 🚀 Como aplicar o projeto no Kubernetes

### 📋 Pré-requisitos

Antes de começar, você precisa ter:

* Kubernetes rodando (minikube, kind ou k3s)
* `kubectl` configurado
* Cluster acessível (`kubectl get nodes` funcionando)

---

### 1️⃣ Subir o Prometheus

```bash
kubectl apply -f prometheus/prometheus-config.yaml
kubectl apply -f prometheus/prometheus-deployment.yaml
kubectl apply -f prometheus/prometheus-service.yaml
```

Verifique se está rodando:

```bash
kubectl get pods
kubectl get svc
```

---

### 2️⃣ Subir o Grafana

```bash
kubectl apply -f grafana/grafana-deployment.yaml
kubectl apply -f grafana/grafana-service.yaml
```

Verifique:

```bash
kubectl get pods
kubectl get svc
```

---

### 3️⃣ Acessar o Grafana

Se estiver usando **NodePort**:

```bash
minikube service grafana
```

Ou via **port-forward**:

```bash
kubectl port-forward svc/grafana 3000:3000
```

Acesse no navegador:

```
http://localhost:3000
```

Credenciais padrão:

* **Usuário:** admin
* **Senha:** admin

---

### 4️⃣ Importar o Dashboard

No Grafana:

1. Menu lateral → **+ Create**
2. Clique em **Import**
3. Faça upload do arquivo:

```text
dashboards/observability-dashboard.json
```

4. Em **Datasource**, selecione **Prometheus**
5. Clique em **Import**

---

### 5️⃣ Validar métricas

No dashboard, confirme se os painéis estão exibindo dados, como:

* CPU Usage
* Memory Usage
* HTTP Requests
* Application Metrics

Se os gráficos estiverem preenchidos, o monitoramento está funcionando corretamente ✅

---

## ✅ Resultado esperado

* Prometheus coletando métricas
* Grafana exibindo dashboards
* Observabilidade básica funcionando em ambiente Kubernetes

---

### 💡 Observação

Este projeto tem foco educacional e demonstra a implementação prática de monitoramento e observabilidade em Kubernetes utilizando Prometheus e Grafana.

📸 Recomenda-se adicionar screenshots do dashboard do Grafana para enriquecer a documentação e o portfólio.

---

## 👤 Autor

**Daniel Viana**
📧 Email: [daniel-viana2127@yahoo.com](mailto:daniel-viana2127@yahoo.com)
🔗 GitHub: [https://github.com/danielviana2127](https://github.com/danielviana2127)
