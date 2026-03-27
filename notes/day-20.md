# Day 20 \- March 27

## Docker
- Software development platform that allows you to create, deploy, and run applications in containers
- Where to store Docker images? Docker repositories, Docker Hub, Amazon ECR
## Docker vs. Virtual Machine
- Docker is a form of virtualization, but not exactly.
- Resources are shared with the host, which allows many containers on one server.
## Docker Containers Management on AWS
- Amazon Elastic Container Service (ECS)
  - Amazon's own container platform
- Amazon Elastic Kubernetes Service (EKS)
  - Amazon's managed Kubernetes service for open-source Kubernetes
- AWS Fargate
  - Amazon's own serverless container platform
  - Works with ECS and EKS
- Amazon ECR
  - Stores container images
## Amazon ECS - EC2 Launch Type
- ECS = Elastic Container Service
- Launching Docker containers on AWS with ECS EC2 launch type means you must launch EC2 tasks on ECS clusters
- EC2 launch type: you must provision and maintain the infrastructure (EC2 instances)
- Each EC2 instance must run the ECS agent to register in the ECS cluster
- AWS takes care of starting and stopping containers
## Amazon ECS - Fargate Launch Type
- You do not provision the infrastructure (no EC2 to manage)
- It is all serverless
- You create a task definition
- AWS just runs the ECS task for you based on the CPU/RAM you need
- To scale, just increase the number of tasks. Simple: no more EC2 instances
## Amazon ECS - IAM Roles for ECS
- EC2 Instance Profile (EC2 launch type)
  - Used by ECS agent
  - Makes API calls to the ECS service
  - Sends container logs to CloudWatch Logs
  - Pulls Docker images from ECR
  - References sensitive data in Secrets Manager or SSM Parameter Store
- ECS Task Role:
  - Allows each task to have a specific role
  - Use different roles for the different ECS services you run
  - Task role is defined in task definition
## Amazon ECS - Load Balancer Integrations
- Application Load Balancer is supported and works for most use cases
- Network Load Balancer is recommended only for high-throughput, high-performance use cases, or to pair it with AWS PrivateLink
- Classic Load Balancer is supported but not recommended (no advanced features, no Fargate)
## Amazon ECS - Data Volumes (EFS)
- Mount EFS file systems on to ECS tasks
- Works for both EC2 and Fargate launch types
- Tasks running in any AZ will share the same data in the EFS file system
- Fargate + EFS = serverless
- Use cases: persistent multi-AZ shared storage for your containers
## ECS Service Auto Scaling
- Automatically increase or decrease the desired number of ECS tasks
- Amazon ECS auto scaling uses AWS Application Auto Scaling
  - ECS service average CPU utilization
  - ECS service average memory utilization - scale on RAM
  - ALB request count per target - metric coming from the ALB
- Target tracking - scale based on target value for a specific CloudWatch metric
- Step scaling - scale based on a specified date/time (predictable changes)
- ECS service auto scaling (task level) is not equal to EC2 auto scaling (instance level)
- Fargate auto scaling is much easier to set up because it is serverless
## ECS Service Auto Scaling EC2 Instances
- Accommodate ECS service scaling by adding underlying ECS instances
- Auto Scaling Group scaling
  - Scale your ASG based on CPU utilization
  - Add EC2 instances over time
- ECS Cluster Capacity Provider
  - Used to automatically provision and scale the infrastructure for your ECS tasks
  - Add EC2 instances when you are missing capacity (CPU, RAM)
## Amazon ECR
- ECR = Elastic Container Registry
- Store and manage Docker images on AWS
- Private and public repositories (Amazon ECR Public Gallery)
- Fully integrated with ECS, backed by Amazon S3
- Access is controlled through IAM
- Supports image vulnerability scanning, versioning, image tags, and image lifecycle policies
## Amazon EKS Overview
- Amazon EKS = Amazon Elastic Kubernetes Service
- It is a way to launch a managed Kubernetes cluster on AWS
- Kubernetes is an open-source system for automatic deployment, scaling, and management of containerized, usually Docker, applications
- It is an alternative to ECS with a similar goal but a different API
- EKS supports EC2 if you want to deploy your worker nodes or Fargate to deploy serverless containers
- Use case: if your company is already using Kubernetes on-premises or in another cloud and wants to migrate to AWS using Kubernetes
## Amazon EKS - Node Types
- Managed Node Groups
  - Creates and manages nodes (EC2 instances) for you
  - Nodes are part of an ASG managed by EKS
  - Supports on-demand or Spot instances
- Self-managed nodes
  - Nodes created by you and registered to the EKS cluster and managed by an ASG
  - You can use a prebuilt AMI - Amazon EKS optimized AMI
  - Supports on-demand or Spot instances
- AWS Fargate
  - No maintenance required: no nodes managed
## Amazon EKS - Data Volumes
- Need to specify a StorageClass manifest on your EKS cluster
- Leverages a Container Storage Interface (CSI)-compliant driver
- Support for...
- Amazon EBS
- Amazon EFS (works with Fargate)
- Amazon FSx for Lustre
- Amazon FSx for NetApp ONTAP
## AWS App Runner
- Fully managed service that makes it easy to deploy web apps and APIs at scale
- No infrastructure experience needed
- Start with your source code or container image
- Automatically builds and deploys the web app
- Automatic scaling, highly available load balancing, and encryption
- VPC access support
- Connects to database, cache, and message queue services
- Use cases: web apps, APIs, microservices, rapid production deployments
## AWS App2Container (A2C)
- CLI tool for migrating and modernizing Java and .NET web apps into Docker containers
- Lift and shift your apps running on-premises, bare metal, virtual machines, or any cloud to AWS
- Accelerate modernization with no code changes for legacy apps
- Generates CloudFormation templates (compute, network)
- Register generated Docker containers to ECR
- Deploy to ECS, EKS, or App Runner
