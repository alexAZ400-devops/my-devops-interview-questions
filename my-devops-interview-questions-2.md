# DevOps Interview Questions

## CI/CD Pipeline

### Can you explain the entire CI/CD pipeline of your project?
In one of my AWS-based projects, I implemented an end-to-end CI/CD pipeline using GitHub, Jenkins, AWS CodePipeline, and CodeDeploy to automate the build, test, and deployment process.
- **Code Commit & Version Control:** Developers pushed their code to GitHub repositories. We followed branching strategies like feature branches and pull requests with integrated code reviews.
- **Build Stage:** Whenever a change was pushed to the main branch, Jenkins triggered a job using webhooks. Jenkins ran unit tests, linting, and built the application (e.g., a Python/Node.js service) into a Docker image.
- **Artifact Storage:** The Docker image was then pushed to Amazon ECR, and the build artifact (e.g., a zipped deployment package or Helm chart) was uploaded to S3.
- **Infrastructure Automation:** We used Terraform and CloudFormation to provision environments like staging and production on-demand, maintaining consistency and version control of infrastructure.
- **Deployment Stage:** Using AWS CodePipeline integrated with CodeDeploy, the latest build was automatically deployed to EC2 instances or EKS clusters, depending on the workload. Canary deployments were used for safer rollouts.
- **Monitoring & Alerts:** Post-deployment, we monitored performance and errors using AWS CloudWatch, and set up alarms and logs for proactive issue detection. For advanced visibility, we integrated Prometheus and Grafana dashboards.

This automated pipeline reduced manual errors, enabled faster delivery, and ensured consistent deployments across environments.

## Git

### What is the difference between git fetch and git pull?
git fetch just fetches the changes and displays, git pull does the 2 commands work i.e. git fetch and git merge

## SonarQube

### What is a code smell in SonarQube?
A code smell is an indicator of a deeper problem in the code—typically poor design or coding practices that might not break functionality but reduce maintainability, readability, or performance.

### What is a quality gate?
A quality gate is a set of conditions that code must meet to be considered acceptable for release.

### What is a quality profile?
A quality profile is a collection of rules (like coding standards, bug detection, etc.) that SonarQube uses to analyze code.

## Maven

### Can you explain the Maven lifecycle?
Maven has three built-in lifecycles:
- **clean:** Removes previous build artifacts.
- **default (Build):** compile, test, package, install, deploy.
- **site:** Generates documentation.

### What is the use of pom.xml?
The core configuration file in Maven. It defines:
- Project info (name, version, packaging)
- Dependencies
- Plugins
- Build settings
- Repositories

### How do you see the dependencies of any application and how do you define them in pom.xml?
Define them using `<dependencies>` block in pom.xml.

### How do you get the dependencies?
Once defined in pom.xml, you run: `mvn clean install`

## Docker

### What is a multi-stage Docker build?
Uses multiple `FROM` statements in a single Dockerfile to separate the build and runtime environments. This helps reduce image size by copying only the necessary artifacts from the build stage to the final image.

### If a Docker image has a huge size, how can you reduce it?
- Use multi-stage builds
- Use a minimal base image like alpine
- Combine RUN commands to reduce layers
- Clean package caches

### What type of base image will you use in a Dockerfile to reduce size?
Use minimal base images like: `alpine`.

## Kubernetes

### Can you explain the Kubernetes architecture?
- **kube-apiserver:** Frontend to the cluster
- **etcd:** Key-value store for cluster state
- **controller-manager:** Handles node/pod life cycles
- **scheduler:** Assigns pods to nodes
- **kubelet:** Communicates with control plane, runs pods
- **kube-proxy:** Manages networking rules
- **container runtime:** Runs containers (e.g., containerd)

### What happens to pods in the worker node if the Kubernetes API server is down?
- Existing running pods continue to run (managed by kubelet and container runtime)
- No new pods can be scheduled or updated until the API server is back
- Health checks and self-healing actions won’t happen

### What is the difference between horizontal and vertical pod autoscaling?
- **Horizontal Pod Autoscaler (HPA):** Increases or decreases the number of pod replicas based on metrics (CPU, memory, custom).
- **Vertical Pod Autoscaler (VPA):** Adjusts CPU/memory resources of a single pod (requests/limits), not the count.

### How do you set metrics in horizontal and vertical pod scaling?
HPA: Requires metrics-server & is set via `kubectl autoscale` or manifest.

### What is a Kubernetes service?
A Service is an abstraction that exposes a set of pods as a network service. It enables stable networking to pods that may change IPs.

### You have a Kubernetes cluster and the node is in a "NotReady" state — what will you check?
- `kubectl describe node <node-name>`
- Node is reachable (network/DNS/firewall)
- kubelet is running
- Disk pressure, memory issues

### You applied a deployment and the pods are stuck in “ContainerCreating” — how will you troubleshoot?
- `kubectl describe pod <pod-name>`
- Image pull issues (e.g., ImagePullBackOff)
- Volume mount failures
- Network/DNS issues
- Missing ConfigMap/Secret

### If a PVC is in a failed state — how would you troubleshoot?
- Check the PVC and PV status: `kubectl get pvc`, `kubectl describe pvc <name>`
- Check for:
  - No matching PV (storage class, access mode, size)
  - Storage backend issues (e.g., EBS volume not available)
  - Errors in events
  - Check logs for kube-controller-manager

## AWS

### Can you explain what a VPC is?
VPC is an isolated environment within cloud provider where we can launch and manage cloud resources like EC2, S3 in a secure and controlled space.

### What is the use of a route table?
Route table is used to control how network traffic is directed within the cloud. Each subnet is associated with one route table. The route table contains destination address or the target.

### How is a public IP address assigned in public subnets?
When launching an EC2, the public IP can be assigned automatically. Then proper security groups and NACL rules must allow inbound/outbound internet traffic.
