Here is an in-depth yet focused explanation of AWS EKS (Elastic Kubernetes Service) tailored for the AWS Solutions Architect Associate (SAA-C03) exam — covering architecture, how it works, networking, security, integrations, pricing, and real-world use cases in a clear, structured, and note-friendly format.


---

🧠 What is AWS EKS?

Amazon Elastic Kubernetes Service (EKS) is a fully managed Kubernetes control plane by AWS.
It lets you run Kubernetes clusters on AWS without managing the control plane or etcd yourself.

✅ Kubernetes = container orchestration system
✅ EKS = AWS-managed Kubernetes platform


---

🚀 Why EKS?

Challenge	Solution

Managing Kubernetes yourself is complex	EKS manages control plane, upgrades, high availability
Scaling workloads securely across clusters	EKS integrates with IAM, VPC, ALB, etc.
Hybrid/Multi-cloud consistency	Runs Kubernetes workloads in AWS or on-prem



---

⚙ Architecture Overview

EKS Architecture
 ┌────────────────────────┐
 │  EKS Control Plane     │  (Managed by AWS)
 │  • API Server          │
 │  • Scheduler           │
 │  • etcd (Cluster DB)   │
 └─────────▲──────────────┘
           │
           ▼
┌────────────────────────┐
│  Node Group (EC2 or Fargate) ─────────────────┐
│  • kubelet runs here                         │
│  • Actual containers/pods live here          │
│  • IAM Roles for Service Accounts (IRSA)     │
└────────────────────────┘

Control Plane is fully managed and deployed across 3 AZs for HA.

Worker Nodes (Data Plane) run containers in EC2 or Fargate.

kubectl CLI is used to manage EKS clusters.



---

🔄 How It Works (Step-by-Step Flow)

1. You create an EKS cluster (control plane is created by AWS).


2. Launch worker nodes (Node Groups):

Either EC2 instances (self-managed or managed node groups)

Or AWS Fargate (serverless compute for pods)



3. Install kubectl and configure aws-iam-authenticator.


4. Deploy workloads (pods, deployments, services, etc.) using Kubernetes YAML manifests.


5. EKS integrates with:

IAM for access control

VPC for networking

CloudWatch for logging

ALB/ELB/NLB for load balancing





---

🧱 Networking in EKS

Component	Details

VPC	Each cluster is launched inside a VPC. Subnets (private/public) are used.
ENI	Each pod gets an Elastic Network Interface (ENI) via VPC CNI plugin.
Security Groups	Applied to worker nodes or load balancers
Load Balancer Controller	Automatically provisions ALB/NLB for Kubernetes Ingress or Services


Pod networking models:

Amazon VPC CNI (default) – Each pod gets a native IP address from VPC subnet.

Alternative CNI like Calico can also be used.



---

🔐 Security in EKS

Feature	Description

IAM for Authentication	IAM users/roles can access EKS using aws-auth ConfigMap
RBAC for Authorization	Role-based access control within Kubernetes
IAM Roles for Service Accounts (IRSA)	Attach IAM permissions to Kubernetes service accounts
Pod Security Policies (deprecated)	Replaced by OPA/Gatekeeper/Kyverno
Secrets Management	Use Kubernetes Secrets or integrate with AWS Secrets Manager / KMS



---

💡 EKS Deployment Models

Model	Description

Managed Node Groups (EC2)	AWS provisions EC2 worker nodes for you
Self-Managed EC2	You manually provision and manage worker nodes
Fargate Profile	Serverless — pods run directly on Fargate, no EC2



---

🧪 Real-World Use Cases

Use Case	Description

Microservices	Deploy scalable, containerized microservices across nodes
Hybrid workloads	Use EKS Anywhere for on-prem clusters
Machine learning	Run ML workloads (e.g., TensorFlow, PyTorch) in containers
CI/CD Pipelines	Integrate with CodePipeline, ArgoCD, GitOps
Gaming	Scalable multiplayer game servers using Kubernetes pods



---

📊 Pricing Overview

You pay for:

1. Control Plane:

$0.10/hour per cluster (regardless of number of nodes)



2. Worker Nodes (EC2 or Fargate):

You pay for EC2/Fargate usage as normal

Additional costs for EBS, Load Balancers, NAT Gateway, etc.




> ❗ Unlike ECS (no control plane fee), EKS charges a fixed hourly fee for cluster operation.




---

🔗 Key Integrations

Service	Role

IAM	Access control via aws-auth
VPC	Networking, IP assignment
CloudWatch	Logs, metrics
ALB/NLB	Load balancing for services
ECR	Private container image registry
AWS WAF + Shield	Protection for Ingress endpoints
KMS	Encrypt secrets and etcd storage
EBS/EFS	Persistent volumes for stateful apps



---

🧠 EKS vs ECS (Exam Tip)

Feature	EKS (Kubernetes)	ECS

Orchestration	Kubernetes	AWS-native
Control Plane	You pay ($0.10/hr)	Free
Flexibility	Very high	AWS opinionated
Portability	Runs anywhere	AWS only (unless using ECS Anywhere)
Learning Curve	Higher	Lower



---

📌 Exam Tips (SAA-C03)

EKS control plane is multi-AZ and highly available by default

Use IAM Roles for Service Accounts (IRSA) to give pod-level permissions

EKS integrates with CloudWatch, ALB, ECR, and IAM

EKS worker nodes can be EC2 or Fargate

You are responsible for data plane security and updates (worker nodes)

Use EKS Add-ons to install common tools (e.g., CoreDNS, kube-proxy)



---

Would you like a one-page summary or YAML deployment flow next?