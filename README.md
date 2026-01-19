# Projeto 4 – Monitoramento e Observabilidade

## 📌 Objetivo

Implementar uma stack completa de **monitoramento e observabilidade** em um ambiente Kubernetes local, simulando um cenário de produção real, utilizando **Prometheus** para coleta de métricas e **Grafana** para visualização.

O foco deste projeto é demonstrar:

* Coleta de métricas reais de uma aplicação
* Integração entre Prometheus e exporters
* Visualização clara de métricas operacionais
* Capacidade de debug e troubleshooting em Kubernetes

---

## 🧱 Stack Utilizada

* **Kubernetes**: Minikube
* **Prometheus**: Coleta e armazenamento de métricas
* **Grafana**: Visualização e dashboards
* **NGINX**: Aplicação monitorada
* **NGINX Exporter**: Exposição das métricas do NGINX para o Prometheus

---

## 📁 Estrutura do Projeto

```
project-04-observability/
├── prometheus/
│   ├── namespace.yaml
│   ├── prometheus-config.yaml
│   ├── prometheus-deployment.yaml
│   ├── prometheus-service.yaml
│   ├── nginx-deployment.yaml
│   ├── nginx-service.yaml
│   └── nginx-exporter.yaml
├── grafana/
│   ├── grafana-deployment.yaml
│   └── grafana-service.yaml
├── dashboards/
│   └── nginx-exporter-dashboard.json
└── README.md
```

---

## 🔍 O Que Está Sendo Monitorado

A aplicação **NGINX** é monitorada através do **nginx-exporter**, que coleta métricas a partir do módulo `stub_status` do NGINX.

### Principais métricas coletadas:

* **nginx_http_requests_total**

  * Total de requisições HTTP processadas

* **nginx_connections_active**

  * Conexões ativas no NGINX

* **nginx_connections_reading / writing / waiting**

  * Estado atual das conexões

* **nginx_connections_accepted / handled**

  * Conexões aceitas e processadas

Essas métricas permitem avaliar **tráfego**, **carga**, **uso de conexões** e **disponibilidade** do serviço.

---

## 📊 Dashboards

O Grafana utiliza o Prometheus como datasource e exibe dashboards com:

* Status da aplicação (Up / Down)
* Total de requisições ao longo do tempo
* Conexões ativas e estados de conexão
* Taxa de processamento de requisições

O dashboard utilizado é baseado nas métricas do **nginx-exporter** e pode ser importado via JSON disponível na pasta `dashboards/`.

---

## 🚨 Alertas Possíveis (Exemplos)

Este projeto não implementa o Alertmanager, mas permite facilmente a criação de alertas como:

* **Alta quantidade de conexões ativas**

  ```promql
  nginx_connections_active > 100
  ```

* **Serviço indisponível**

  ```promql
  up{job="nginx"} == 0
  ```

* **Queda brusca de requisições**

  ```promql
  rate(nginx_http_requests_total[5m]) == 0
  ```

Esses alertas seriam úteis para identificar falhas, sobrecarga ou indisponibilidade do serviço.

---

## ▶️ Como Executar o Projeto

1. Iniciar o Minikube:

   ```bash
   minikube start
   ```

2. Criar o namespace e aplicar os manifests:

   ```bash
   kubectl apply -f prometheus/
   kubectl apply -f grafana/
   ```

3. Verificar os pods:

   ```bash
   kubectl get pods -n observability
   ```

4. Acessar os serviços via port-forward:

   * Prometheus:

     ```bash
     kubectl port-forward -n observability svc/prometheus 9090:9090
     ```

   * Grafana:

     ```bash
     kubectl port-forward -n observability svc/grafana 3000:3000
     ```

5. Acessar o Grafana:

   * URL: [http://localhost:3000](http://localhost:3000)
   * Usuário padrão: `admin`
   * Senha padrão: `admin`

---

## 🧠 Aprendizados do Projeto

* Funcionamento interno do Prometheus e scrape de métricas
* Uso de exporters para observabilidade
* Integração Prometheus + Grafana
* Debug de Services, Endpoints e DNS no Kubernetes
* Visualização de métricas reais em tempo real

---

## 🏁 Conclusão

Este projeto demonstra uma implementação **realista e funcional** de observabilidade em Kubernetes, abordando desafios comuns encontrados em ambientes de produção e consolidando fundamentos essenciais para atuação em **DevOps / SRE**.

---

📌 **Autor**: Daniel Viana

