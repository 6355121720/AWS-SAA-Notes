Here’s a detailed, in-depth set of AWS ECS (Elastic Container Service) notes tailored for SAA-C03 (AWS Certified Solutions Architect – Associate) exam preparation:


---

✅ AWS ECS (Elastic Container Service) – In-Depth Notes for SAA-C03


---

🧠 What is ECS?

Amazon ECS is a fully managed container orchestration service that helps run and scale Docker containers on AWS.


---

📦 ECS Core Concepts

Term	Description

Cluster	Logical grouping of container instances or Fargate tasks.
Task Definition	Blueprint for your application. Includes Docker image, CPU, memory, port mappings, etc.
Task	Instantiation of a Task Definition, i.e., a running containerized app.
Service	Maintains a specified number of running tasks; supports load balancing and auto scaling.
Container Instance	EC2 instance that runs ECS agent and is part of an ECS Cluster (EC2 launch type only).
ECS Agent	Runs on container instances to communicate with ECS control plane.



---

🚀 Launch Types

Launch Type	Description

EC2	You manage EC2 instances; more control over infra.
Fargate	Serverless; AWS provisions and manages infrastructure.


> ✅ Fargate is preferred for serverless and easier ops. You only define task-level CPU/RAM.




---

📄 Task Definitions

JSON file that includes:

Docker image

CPU & Memory

Environment variables

Port mappings

IAM Role (Task Execution Role)

Logging config (CloudWatch)

Volumes




---

🛠 ECS Services

Ensures high availability by keeping desired number of tasks running.

Supports Load Balancing:

ALB/NLB integration

Dynamic port mapping (especially with ALB + Fargate)


Auto Scaling:

Scale task count via CloudWatch alarms.

Scale EC2 instances using Auto Scaling Groups (for EC2 launch type).




---

🌐 Networking Modes

Mode	Description

Bridge	Traditional Docker networking
Host	Uses host’s network stack (high performance)
awsvpc	Each task gets its own ENI (used in Fargate) – Best isolation


> ✅ awsvpc is the default and only mode for Fargate.




---

🔐 IAM in ECS

Task Role: IAM Role the task assumes to interact with other AWS services (S3, DynamoDB, etc.)

Task Execution Role: Needed to pull container image from ECR, send logs to CloudWatch.



---

🗃 ECS with Load Balancers

ECS can register tasks with:

Application Load Balancer (ALB): Layer 7, supports path-based routing.

Network Load Balancer (NLB): Layer 4, for high-performance networking.


Target group registration:

ECS registers tasks as targets.

Dynamic port mapping supported with ALB.




---

⚙ ECS Auto Scaling

1. Service Auto Scaling

Maintains number of tasks.

Triggered via CloudWatch metrics (e.g., CPU utilization).



2. Cluster Auto Scaling

For EC2 launch type.

Ensures EC2 instances scale in/out to meet capacity needs.





---

📊 Monitoring & Logging

CloudWatch:

CPU/Memory/Task counts

Log collection via awslogs or Fluent Bit


Container Insights for detailed performance data



---

🔁 Blue/Green Deployments (with CodeDeploy)

ECS integrates with AWS CodeDeploy for blue/green deployments.

Supported only with:

ECS on Fargate

ECS on EC2 with ALB




---

📌 Use Cases & Best Practices

Use Case	Recommended Setup

Simple app with serverless infra	Fargate + ALB
High performance, custom AMIs	EC2 launch type
Fine-grained cost optimization	Fargate Spot
Complex networking or GPU	EC2
Multiple microservices	Fargate + awsvpc
On-prem integration	ECS Anywhere



---

🧪 Exam Tips

ECS with Fargate requires awsvpc network mode and task execution role.

Use Fargate Spot to save cost with interruption-tolerant apps.

ECS Service Auto Scaling uses CloudWatch alarms.

Use dynamic port mapping with ALB to run multiple tasks on same EC2 host.

ECS Anywhere extends ECS to on-prem servers.

Task Role ≠ Task Execution Role – understand their difference!



---

If you’d like flashcards, diagrams, or a sample scenario breakdown (with architecture), let me know.