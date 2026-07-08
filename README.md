# EFK Stack on Kubernetes

Centralised logging solution on Kubernetes using Elasticsearch, Fluentd, and Kibana. Fluentd runs as a DaemonSet to collect logs from all pods automatically, without per-application configuration.

## Architecture

```
All pods (all nodes)
       ↓
Fluentd DaemonSet
(one per node, auto-collects all pod logs)
       ↓
Elasticsearch StatefulSet
(persistent storage, indexed)
       ↓
Kibana Deployment
(search and visualise at :5601)
```

## Deployment

```bash
git clone https://github.com/mimii020/EFK-on-kBs.git
cd EFK-on-kBs

kubectl create namespace logging
kubectl apply -f elasticsearch.yaml -n logging
kubectl wait --for=condition=ready pod -l app=elasticsearch -n logging --timeout=120s
kubectl apply -f fluentd.yaml -n logging
kubectl apply -f kibana.yaml -n logging

minikube service kibana -n logging
```

## Prerequisites

- minikube with at least 4GB RAM (Elasticsearch requirement)
- kubectl

## Useful Kibana Queries

```
kubernetes.namespace_name: "default"
log: "ERROR"
kubernetes.pod_name: "my-app-*"
```

## Stack

Elasticsearch · Fluentd · Kibana · Kubernetes (minikube) · Docker

---
**Imen Abidi** · [GitHub](https://github.com/mimii020) · [LinkedIn](https://linkedin.com/in/imen-abidi)
