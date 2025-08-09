# Kubernetes Ingress with MetalLB LoadBalancer

This document describes the steps followed to deploy **MetalLB** as a LoadBalancer for a Kubernetes cluster and expose services/Ingress via an external IP address.

---

## 1. Why MetalLB?
Kubernetes by default does not provide an implementation for **LoadBalancer** type services on bare-metal clusters.  
MetalLB is a load balancer implementation for bare-metal Kubernetes clusters, allowing you to assign **external IPs** from a predefined pool.

---

## 2. Prerequisites
- A working Kubernetes cluster (bare-metal or VM-based)
- `kubectl` access to the cluster
- A free range of IP addresses on your local network that are **not in use** and **reachable** from your machine

---

## 3. Install MetalLB

### 3.1 Deploy the MetalLB components
```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.9/config/manifests/metallb-native.yaml


