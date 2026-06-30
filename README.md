# Kubernetes Services

This repository demonstrates the different types of **Kubernetes Services** using a single Nginx Deployment.

The goal of this repository is to help beginners understand **why Services are needed**, how they work, and how different Service types expose applications.

---

# Repository Structure

```
.
├── deployment.yaml
├── clusterip-service.yaml
├── nodeport-service.yaml
├── loadbalancer-service.yaml
├── images/
│   └── kubernetes-services.png
└── README.md
```

---

# Kubernetes Service Architecture


images/kubernetes-services.png

---

# Why Do We Need a Service?

In Kubernetes, applications run inside **Pods**.

Each Pod receives its own IP address.

Since Pods are **ephemeral**, they can be recreated at any time, causing their IP addresses to change.

Without a Service:

- Clients need to know every Pod IP.
- Pod IPs change after Pod recreation.
- Traffic management becomes difficult.
- Applications are not scalable.

A **Service** solves this problem by providing a **stable endpoint** for a group of Pods.

Instead of connecting directly to Pods, applications connect to the Service.

The Service automatically forwards traffic to healthy Pods selected using labels.

---

# Service Types Covered

This repository demonstrates:

- ClusterIP
- NodePort
- LoadBalancer

---

# Deploy the Application

Create the Deployment.

```bash
kubectl apply -f deployment.yaml
```

Verify the Pods.

```bash
kubectl get pods -o wide
```

---

# 1. ClusterIP Service

Apply the Service.

```bash
kubectl apply -f clusterip-service.yaml
```

Verify.

```bash
kubectl get svc
```

Expected output:

```
NAME                 TYPE        CLUSTER-IP
nginx-clusterip      ClusterIP   10.x.x.x
```

### Use Case

- Internal communication between applications.
- Default Service type.
- Accessible only within the Kubernetes cluster.

---

# 2. NodePort Service

Apply the Service.

```bash
kubectl apply -f nodeport-service.yaml
```

Verify.

```bash
kubectl get svc
```

Expected output:

```
NAME               TYPE       PORT(S)
nginx-nodeport     NodePort   80:30080/TCP
```

Access the application.

```
http://<Node-IP>:30080
```

### Use Case

- Development environments.
- Testing applications.
- Simple external access without a cloud Load Balancer.

---

# 3. LoadBalancer Service

Apply the Service.

```bash
kubectl apply -f loadbalancer-service.yaml
```

Verify.

```bash
kubectl get svc
```

Example output:

```
NAME                    TYPE           EXTERNAL-IP
nginx-loadbalancer      LoadBalancer   35.xx.xx.xx
```

Access the application.

```
http://<External-IP>
```

### Use Case

- Production workloads.
- Cloud environments (AWS, Azure, GCP).
- Automatically provisions a cloud Load Balancer.

---

# Useful Commands

View Pods

```bash
kubectl get pods
```

View Services

```bash
kubectl get svc
```

View Endpoints

```bash
kubectl get endpoints
```

Describe a Service

```bash
kubectl describe svc <service-name>
```

Delete a Service

```bash
kubectl delete -f clusterip-service.yaml
```

Delete the Deployment

```bash
kubectl delete -f deployment.yaml
```

---

# How Services Work

1. A Deployment creates Pods.
2. Each Pod is assigned a unique IP address.
3. The Service uses a **selector** to identify Pods with matching labels.
4. Kubernetes automatically creates Endpoints for those Pods.
5. Clients send requests to the Service.
6. The Service forwards traffic to one of the healthy Pods.
7. Even if Pods are recreated, the Service endpoint remains unchanged.

---

# Learning Outcome

After completing this example, you will understand:

- Why Kubernetes Services are required.
- How Services discover Pods using labels.
- The purpose of ClusterIP, NodePort, and LoadBalancer.
- How traffic flows from a client to a Pod.
- Why Services provide a stable endpoint despite changing Pod IPs.

---

⭐ If this repository helped you understand Kubernetes Services, consider giving it a star!