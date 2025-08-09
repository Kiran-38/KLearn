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
```
---

### 4. Find Free IP Range
```bash
ip addr show   # find subnet (e.g., 172.30.1.0/24)
nmap -sn 172.30.1.0/24  # find used IPs
```
Pick a free range (e.g., 172.30.1.240-172.30.1.250).

----

### 5. Configure MetalLB Address Pool
```bash
kubectl apply -f metallb-config.yaml
```

---

### 6. Verify LoadBalancer IP
```bash
kubectl get svc -n ingress-nginx
```
Should show EXTERNAL-IP from your pool:
```bash
ingress-nginx-controller   LoadBalancer   10.96.x.x   172.30.1.240   80:xxxx/TCP,443:xxxx/TCP
```

### 7. Access via Ingress
```bash
curl http://172.30.1.240/nginx
```
Optional: map to a hostname in /etc/hosts:
```bash
172.30.1.240 myapp.local
```
Then:
```bash
curl http://myapp.local/nginx
```
