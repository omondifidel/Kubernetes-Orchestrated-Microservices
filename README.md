🚀 SkyBank: Kubernetes-Orchestrated Microservices
A cloud-native evolution of the SkyBank ecosystem, engineered for scalability and high availability using Kubernetes.

🏗️ Architectural Evolution
This project demonstrates a successful architectural transition from a monolithic core to a decoupled, containerized system.

Frontend Service: An Nginx-backed web interface.

Backend API: A FastAPI/Flask service handling core banking logic.

Database Pod: A persistent PostgreSQL instance managed within the cluster.

Service Discovery: Internal cluster communication managed via Kubernetes Services (backend-service, db-service).

🛠️ The DevOps & SRE Case Study
Deploying microservices isn't just about writing code; it's about managing the environment. Here is how I navigated the deployment bottlenecks:

1. Observability & Log Analysis
Before the system was "Healthy," I had to ensure the containers were starting correctly.

Action: Used kubectl logs to monitor the Nginx and Backend startup sequences.

Insight: Verified that Nginx was correctly sourcing local resolvers and worker processes were starting on Alpine Linux.

2. Solving the "Site Can't Be Reached" Bottleneck
One of the primary challenges was external access. Initially, the site timed out with an ERR_CONNECTION_TIMED_OUT.

The Issue: The Kubernetes NodePort service was active, but the environment (Minikube) required a specific tunnel to expose the IP 192.168.49.2.

The Solution: I implemented Port Forwarding (kubectl port-forward) to bridge the cluster's internal port 80 to my local machine's port 8080.

3. Inter-Pod Communication & Health Checks
I built a custom System Health Check dashboard to monitor pod status in real-time.

Database Integrity: Verified table creation and manual data insertion directly within the running pod using kubectl exec -it to access the PostgreSQL CLI.

Service Mapping: Successfully mapped the frontend service to NodePort 30080, allowing the frontend to communicate with the backend API seamlessly across the cluster network.

📊 Infrastructure Gallery
The Dashboard (Success)
The final UI showing "System Status: Alive" and the real-time logs of inter-pod communication.

The Troubleshooting Phase
Monitoring pod health and service IP assignments to resolve connection issues.

Database Verification
Directly interacting with the persistent layer to ensure data atomicity.

🚀 Technical Commands Used
Cluster Status: minikube status & kubectl get pods.

Service Exposure: minikube service frontend --url.

Connectivity Fix: kubectl port-forward deployment/sky-backend 8000:8000.

Resource Inspection: kubectl describe svc frontend.

🧠 Lessons Learned
This project reinforced the importance of Network Policies and Port Mapping in a microservices architecture. It shifted my perspective from "Application Developer" to "Systems Architect," focusing on how components talk to each other under stress.
