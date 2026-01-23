# Project 04 – Observability

## Objetivo

Implementar monitoramento e observabilidade em um ambiente Kubernetes utilizando Prometheus e Grafana, com foco em métricas reais disponíveis em um setup simplificado de estudos.

Este projeto demonstra a criação, organização, persistência e versionamento de dashboards no Grafana.

---

## Stack Utilizada

* Kubernetes (cluster local)
* Prometheus
* Grafana

---

## Estrutura do Projeto

```text
project-04-observability/
├── dashboards/
│   └── observability-dashboard.json
├── prometheus/
│   └── (arquivos de configuração do Prometheus)
├── grafana/
│   └── (configurações do Grafana, se aplicável)
└── README.md
```

---

## Acesso ao Grafana

O Grafana está exposto via port-forward:

```bash
kubectl port-forward -n observability svc/grafana 3000:3000
```

Acesso pelo navegador:

```
http://localhost:3000
```

---

## Métricas Monitoradas

Devido ao uso de um ambiente Kubernetes simplificado, sem cAdvisor e node-exporter habilitados, foram utilizadas métricas compatíveis com o setup disponível.

As métricas monitoradas incluem:

* **Status dos targets:**

  ```promql
  up
  ```

* **Uso de CPU por processo:**

  ```promql
  rate(process_cpu_seconds_total[5m])
  ```

* **Uso de memória por processo:**

  ```promql
  process_resident_memory_bytes
  ```

* **Quantidade de séries ativas no Prometheus:**

  ```promql
  prometheus_tsdb_head_series
  ```

Essas métricas garantem visibilidade sobre a saúde do Prometheus e dos processos monitorados.

---

## Dashboards

O dashboard do Grafana foi criado manualmente via interface gráfica, salvo e exportado em formato JSON para versionamento.

Arquivo disponível em:

```
dashboards/observability-dashboard.json
```

### Importar o Dashboard no Grafana

1. Acesse o Grafana
2. Menu lateral → **Create → Import**
3. Faça upload do arquivo JSON ou cole o conteúdo
4. Selecione o datasource **Prometheus**
5. Clique em **Import**

O dashboard será carregado automaticamente.

---

## Resultado

O projeto entrega:

* Observabilidade funcional
* Dashboard persistido e reprodutível
* Versionamento de dashboards como código
* Base sólida para evolução com métricas por pod, namespace e container

---

## Próximos Passos

* Habilitar node-exporter
* Habilitar kube-state-metrics
* Adicionar métricas por pod e namespace
* Criar alertas no Prometheus

---

## Autor

**Daniel Viana**
GitHub: [https://github.com/danielviana2127](https://github.com/danielviana2127)
