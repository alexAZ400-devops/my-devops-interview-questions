# DevOps and Kubernetes Interview Notes

## CI/CD Pipeline Overview

### Can you explain the end-to-end CI/CD pipeline in your project?
We had created CI/CD pipeline & automated infrastructure provisioning and configuration management of Amazon Connect using GitHub, AWS CodePipeline, CloudFormation, S3, and DynamoDB Streams.
In GitHub the templates.yaml file stores Amazon Connect configurations like Queues, Quick Connects, and Routing Profiles.
When changes are committed and pushed to github, CodePipeline triggers execution across several stages: 
Source (retrieves files and performs code quality, security, and testing checks via SonarQube, Trivy, and linting tools), 
Build (validates CloudFormation syntax with cfn-lint), 
Test (executes CloudFormation Change Sets and verifies integrations), 
Approval for Production (manual review for safe deployment), and 
Deploy (applies CloudFormation changes, updates agent configurations stored in S3, and reflects updates dynamically via DynamoDB Streams).
Contact flows, user management, and configurations are handled manually within the Amazon Connect console.
We made sure this pipeline ensures automated & scalable updates to Amazon Connect resources while maintaining security, validation, and real-time synchronization across services.

## Kubernetes Concepts

### What is the use of ConfigMap in Kubernetes?
Stores configuration data (key-value pairs) separate from containerized applications. Allows dynamic updates without restarting pods.

### What kind of data is stored in a ConfigMap?
- Environment variables
- Application configuration files
- Command-line arguments

### What is the difference between StatefulSet and Deployment?
- **StatefulSet**: For stateful applications (e.g., MySQL, PostgreSQL, Shopping cart).
- **Deployment**: For stateless apps (e.g., API server, frontend app).

### What do you do if a pod is stuck in CrashLoopBackOff?
```bash
kubectl logs pod-name
kubectl describe pod pod-name
```
Fix misconfigurations like memory limits or environment variables.

### What is an Ingress in Kubernetes?
Manages external access to services using HTTP/HTTPS routing.

### What is a DaemonSet?
Ensures a pod runs on every node in the cluster (e.g., for monitoring agents).

### What is the difference between PV and PVC?
- **PV (PersistentVolume)**: Pre-provisioned storage.
- **PVC (PersistentVolumeClaim)**: Requests storage.

### What is the difference between ClusterIP and NodePort service types?
- **ClusterIP**: Internal-only
- **NodePort**: Externally accessible via node's IP

### What is HPA (Horizontal Pod Autoscaler)?
Automatically scales pods based on CPU/memory usage.

### What is RBAC in Kubernetes?
Role-Based Access Control - Restricts permissions.

### What are the different types of RBAC roles?
- **Role**: Namespace-specific
- **ClusterRole**: Cluster-wide
- **RoleBinding**: Binds Role to users in a namespace
- **ClusterRoleBinding**: Binds ClusterRole to all namespaces

### Kubernetes Architecture

#### Control Plane
- **API Server**: Handles requests.
- **Scheduler**: Assigns pods to nodes.
- **Controller Manager**: Manages controllers.
- **etcd**: Stores cluster state.

#### Worker Node
- **Kubelet**: Ensures pods are running.
- **Kube Proxy**: Manages networking.
- **Container Runtime**: Runs containers (e.g., containerd).

### Container Runtimes in Kubernetes
- Docker (deprecated)
- containerd (preferred)

## AWS & VPC Concepts

### What is a VPC?
A Virtual Private Cloud is an isolated section of AWS with customizable networking.

### VPC Components
- Subnets (Public & Private)
- Route Tables
- Internet Gateway (IGW)
- NAT Gateway / NAT Instance
- Security Groups & NACLs

### How many internet gateways can a VPC have?
Only one IGW per VPC.

### What goes in a private subnet?
- Databases (RDS)
- Backend services (APIs)
- Internal tools
- Batch systems

### How does a private EC2 get internet access?
- NAT Gateway
- NAT Instance
- VPC Peering / Transit Gateway

### What is an Elastic IP (EIP)?
Static public IP assignable to AWS resources.

### Where can EIPs be assigned?
- EC2 Instances
- NAT Gateways
- Classic ELB

### Advantage of EIP over public IP?
Static and reassignable

## Kubernetes Deployment Strategies

### What deployment methods do you use?
- **Blue-Green**: Traffic switch between environments.
- **Canary**: Gradual release.
- **Rolling Update**: Batch-wise replacement.

## Jenkins & Git

### How to integrate SonarQube with Jenkins?
- Install SonarQube server and plugin in Jenkins.
- Configure SonarQube server in Jenkins Global Tool Configuration.

### What is a Sonar scanner?
Tool that sends code to SonarQube for static analysis.

### What if SonarQube server is slow?
- Code analysis steps that depend on SonarQube (e.g., during the Source or Build stage) will take longer or timeout.
- If the server doesn’t respond within the expected time, tools like Jenkins, CodeBuild, or GitHub Actions may fail the step.
- If your pipeline is configured to require successful SonarQube analysis before proceeding to later stages (e.g., test or deploy), a slow SonarQube server can block the whole deployment.

### Jenkins Declarative Pipeline Stages
- `pipeline`, `agent`, `stages`, `steps`, `post`

### Git Essentials

#### What is a Git branch?
Isolated line of development. Create using:
```bash
git checkout -b branch-name
```

#### Use of .gitignore?
Prevents unwanted files from being tracked.

#### Can Git handle large files?
Yes, with Git LFS.

#### How to reset last commit?
```bash
git reset --soft HEAD~1
git reset --mixed HEAD~1
git reset --hard HEAD~1
```

## Docker Essentials

### How to check Docker containers?
- Running: `docker ps`
- Stopped: `docker ps -a`

### Docker Volumes
Persist data beyond container lifecycle.
```bash
docker volume create my_volume
docker run -d --name container1 -v my_volume:/data busybox
docker run -d --name container2 -v my_volume:/data busybox
```

### Dockerfile Example
```dockerfile
FROM ubuntu:latest
WORKDIR /app
COPY . .
RUN apt-get update && apt-get install -y curl
CMD ["python", "app.py"]
```

### CMD vs ENTRYPOINT
- **CMD**: Default, override-able
- **ENTRYPOINT**: Not override-able

### Run Docker with ports
```bash
docker run -d -p 8080:80 myimage
```

### Inside Docker container
```bash
docker exec -it container_id bash
```

### Check image vulnerabilities
```bash
trivy image myimage
docker scan myimage
```

### Reduce Docker image size
- Use Alpine/distroless
- Remove layers
- Use multi-stage builds

### Multi-stage Dockerfile Example
```dockerfile
FROM golang:1.20 AS builder
WORKDIR /app
COPY . .
RUN go build -o myapp

FROM alpine:latest
WORKDIR /app
COPY --from=builder /app/myapp .
CMD ["./myapp"]
```

### Benefits of smaller images
- Faster builds/deploys
- Lower bandwidth
- Improved security
- Less registry storage

## Maven Essentials

### Why dependencies in pom.xml?
Define required libraries for build.

### Components in pom.xml
- **Project Coordinates**
- **Dependencies**
- **Build Plugins**
- **Repositories**
- **Profiles**

### What is a Maven plugin?
- Compiler Plugin: Java compilation
- Surefire Plugin: Unit tests
- Assembly Plugin: Distributions

### Build a Maven project
```bash
mvn clean install
```

### Maven Lifecycle Stages
- `clean`, `validate`, `compile`, `test`, `package`, `verify`, `install`, `deploy`
