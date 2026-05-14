# 🚀 SkyBank: Kubernetes-Orchestrated Microservices

**A resilient, cloud-native banking ecosystem demonstrating advanced container orchestration, service discovery, and SRE troubleshooting methodologies.**

---

## 🏛️ System Architecture

SkyBank has been evolved from a monolith into a decoupled microservices architecture, managed within a Kubernetes (k8s) cluster. This ensures that each component can be scaled independently and remains fault-tolerant.

* **Frontend (Nginx/React):** Serves the customer dashboard and handles user interactions.
* **Backend API (Python/Flask):** The core engine managing the transaction ledger, account logic, and authentication.
* **Database (PostgreSQL):** A persistent stateful layer for high-integrity financial data.
* **Orchestration:** Managed via **Minikube** with custom manifests for Deployments, Services, and PersistentVolumes.

---

## 🛠️ The SRE Journey: Debugging & Infrastructure Resilience

Deploying to Kubernetes introduced real-world infrastructure challenges. Below is the documented process of moving from a failing deployment to a healthy cluster.

### **1. Resolving Connectivity Bottlenecks (The "Port Forwarding" Fix)**

**The Challenge:** After deployment, the frontend was active in the cluster but inaccessible via the browser, resulting in a "Site Can't Be Reached" error.
**The Investigation:** Checked the Service status:

```bash
kubectl get svc frontend

```

**The Root Cause:** The environment (Minikube/Codespaces) was not automatically routing external traffic to the NodePort.
**The Resolution:** Implemented **manual port forwarding** to bridge the cluster network to the host machine.

* **Action:** `kubectl port-forward svc/frontend 8080:80`
* **Verification:** Accessed the UI successfully at `localhost:8080`.

### **2. Observability through Log Analysis**

**The Challenge:** Initial inter-pod communication between the Backend and Database was failing.
**The Investigation:** I dove into the pod logs to identify the handshake failure.

* **Command:** `kubectl logs -f <backend-pod-name>`
**The Insight:** The logs revealed a "Connection Refused" error because the backend was looking for a local DB rather than the Kubernetes Service DNS.
**The Resolution:** Updated the backend environment variables to point to the internal k8s DNS: `db-service:5432`.

### **3. Database Integrity & Internal Inspection**

**The Action:** To verify data persistence, I performed an "in-pod" inspection of the PostgreSQL layer.

* **Command:** `kubectl exec -it <db-pod-name> -- psql -U postgres`
**The Result:** Confirmed that the banking schemas and initial user tables were correctly provisioned during the automated startup sequence.

---

## 📊 Infrastructure Status Gallery

### **✅ System Heartbeat: Healthy**

The Kubernetes dashboard confirms all pods (Frontend, Backend, DB) are in a **Running** state with stable restarts.

### **🛠️ Active Troubleshooting**

Identifying service ports and mapping the frontend URL for external access.

### **📋 Service Logs**

Monitoring the real-time health and worker processes of the Nginx frontend.

---

## 🚀 Deployment Guide

### **1. Initialize Cluster**

```bash
minikube start

```

### **2. Deploy Resources**

```bash
kubectl apply -f k8s/db-deployment.yaml
kubectl apply -f k8s/backend-deployment.yaml
kubectl apply -f k8s/frontend-deployment.yaml

```

### **3. Bridge Connectivity**

If working in a restricted environment like Codespaces, forward the ports:

```bash
kubectl port-forward deployment/frontend 8080:80

```

---

## 🧠 SRE Skills Demonstrated

* **Container Orchestration:** Expertise in managing pod lifecycles and deployments.
* **Network Debugging:** Solving complex routing issues using NodePorts and Port-Forwarding.
* **System Observability:** Utilizing `kubectl logs` and `kubectl describe` for root-cause analysis.
* **Infrastructure as Code:** Managing cluster state through declarative YAML manifests.

---
SCREENSHOTS

Site Cant be reached
<img width="1365" height="681" alt="Site error" src="https://github.com/user-attachments/assets/8538219c-1c70-4d86-bc16-88a965230865" />

Getting Pods 
<img width="734" height="172" alt="pods" src="https://github.com/user-attachments/assets/2ddf792c-a854-4061-9ded-7bf7c269dfb1" />

Reading Logs
<img width="1007" height="322" alt="logs " src="https://github.com/user-attachments/assets/a3f2f316-8477-4f12-93de-4358d7c6a790" />

Checking Services 
<img width="1365" height="728" alt="inspect" src="https://github.com/user-attachments/assets/f76fc1ca-d272-43ec-a64f-3c5f5844f133" />

Getting Frontend Service URL 
<img width="1365" height="719" alt="Kuber Port Fowarding " src="https://github.com/user-attachments/assets/74be0c76-0b88-48ba-b216-9c1c6fa1518c" />
<img width="1098" height="389" alt="Getting the frontend" src="https://github.com/user-attachments/assets/b83e8981-40ff-4761-88eb-4b3d24e42427" />

Port Fowarding 
<img width="1135" height="400" alt="forward" src="https://github.com/user-attachments/assets/46ea5798-fc69-4ec5-8be7-9cc2a2407b2f" />

Homepage
<img width="1365" height="721" alt="Kuber Home" src="https://github.com/user-attachments/assets/31f5e1be-a2c2-41d3-ac8a-b56591261dcc" />

DB
<img width="1345" height="723" alt="db" src="https://github.com/user-attachments/assets/e3d22da8-3c27-4f3b-b944-0d0ed1c458dd" />
