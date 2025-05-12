# AWS DevOps Interview Questions

### 1. What is a parameter group in Amazon RDS, and what is its purpose?
A parameter group in RDS is a collection of database engine configuration settings that control the behavior of an RDS instance. It allows you to fine-tune performance aspects such as memory usage, query optimization, and logging.

### 2. What are ECS (Elastic Container Service) and ECR (Elastic Container Registry) in the context of containerization?
Amazon ECS (Elastic Container Service) is a managed container orchestration service.
Amazon ECR (Elastic Container Registry) is a scalable registry for storing, managing, and deploying Docker container images.

### 3. How do I create Docker images and push them to Amazon ECR?
docker build -t my-app .
docker tag my-app:latest <account-id>.dkr.ecr.us-east-1.amazonaws.com/my-app:latest
docker push <account-id>.dkr.ecr.us-east-1.amazonaws.com/my-app:latest

### 4. In a multi-account setup using AWS CloudFormation, how can I create a single IAM role across all accounts?
Use AWS Organizations and CloudFormation StackSets to deploy an IAM role across multiple accounts in a unified manner.

### 5. If I have a 450MB Docker image, what are the best practices to optimize and reduce its size?
Use multi-stage builds to reduce unnecessary dependencies.
Select minimal base images like alpine.
Remove unnecessary files using .dockerignore.
Compress layers using the docker build --squash flag

### 6. What is the difference between Docker and Kubernetes in container orchestration?
Docker is a containerization tool, while Kubernetes is an orchestration tool for managing containerized applications across multiple nodes.

### 7. What is a pod in Kubernetes, and how does it function?
A Pod is the smallest deployable unit in Kubernetes, encapsulating one or more containers that share network and storage resources.

### 8. What is the difference between a container and a pod in Kubernetes?
A Container is a runtime instance of an image.
A Pod is a Kubernetes object that can contain multiple containers that need to run together.

### 9. How is load balancing achieved within pods in Kubernetes?
Kubernetes services like ClusterIP, NodePort, and LoadBalancer help distribute traffic across pods.
Ingress controllers enable HTTP/S routing.

### 10. How can I fetch all EC2 instances across all AWS accounts and regions?
Use AWS Organizations + IAM cross-account roles + STS AssumeRole + scripting.

### 11. How can I fetch all EC2 instances from all accounts within a specific AWS Organizational Unit (OU)?
Use AWS Organizations APIs to first get the list of accounts in the OU, then use the same cross-account assume-role method to query EC2.

### 12. If two VPCs in different AWS accounts have the same CIDR block, can VPC peering be established between them, and what are the alternatives?
No, VPC Peering cannot be established if the VPCs have overlapping CIDR blocks.
Alternatives:
VPC Lattice (newer solution for service-to-service connectivity)
Transit Gateway with NAT or Route Translation
VPN or AWS PrivateLink (if service-specific access is required)
Re-IP one VPC (change CIDR, if feasible)

### 13. We usually see a 2/2 status check on EC2 instances; what does it mean if we now see a 3/3 status check?
System+Instance check, if 3/3 is there that means AWS has launched new monitoring check.

### 14. If an EC2 instance is hosted in the management account and we create an AMI backup, how can we share that AMI with child accounts?
can share the AMI manually or via CLI, Also share associated EBS snapshots by modifying their permissions

### 15. After sharing an AMI with child accounts and launching the instance, it is launching and terminating automatically. What could be the reason for this behavior?
Corrupt AMI (incomplete or broken boot volume)
Missing EBS snapshot permissions
Startup script failure (user-data)
Unsupported instance type or availability zone
IAM permissions or misconfigured launch template

### 16. How can we identify if the AMI we shared has an issue that might be causing the instance to launch and terminate automatically?
Enable EC2 termination protection temporarily.
Launch an instance from the AMI in your own account.
Check System Logs and Instance Console Output.
Use aws ec2 get-console-output to fetch logs.
Validate user-data, boot logs, and network settings.
Confirm that the base snapshot is healthy and permissions are correctly set.

### 17. What are the steps involved in configuring a 3-tier architecture in AWS?
Presentation Layer (Web Tier): EC2, ELB, or CloudFront, Hosted in public subnet
Application Layer: Backend APIs, EC2 in private subnet, Auto Scaling + ALB, Security group allows access only from Web tier
Database Layer: Amazon RDS/Aurora in private subnet, Security group allows only app tier access, Enable multi-AZ for HA
NAT Gateway for outbound internet
IAM roles for least privilege
Monitoring with CloudWatch and Logs
VPC with proper routing & subnetting

### 18. If we are trying to download or save an RDS backup to an S3 bucket but RDS is not connecting to S3, how can we troubleshoot this issue?
Ensure RDS has proper IAM Role with AmazonS3FullAccess or AmazonS3ReadOnlyAccess as needed.
RDS Role must be associated with the instance or snapshot export job.
S3 bucket must be in the same region as RDS snapshot.
Check bucket policy – ensure it allows access from RDS.
If using VPC Endpoint for S3, ensure it's reachable.
Verify KMS permissions if the bucket or snapshot is encrypted.
Review CloudTrail logs for access denied or role assumption issues.

### 19. Difference between EFS and EBS
Use EFS when you need a shared, scalable file system that multiple EC2 instances can access simultaneously.
Use EBS when you need high-performance, single-instance storage like for databases or application data.

### 20. What challenges did you face during the integration, and how did you resolve them?
Challenge: Webhook triggers not firing correctly due to network issues.
Resolution: Configured retry mechanisms and network diagnostics to ensure stable webhook delivery.
Challenge: Intermittent build failures due to dependency issues.
Resolution: Implemented caching mechanisms and isolated builds to ensure consistent environment setups.

### 21. How do you ensure that your Jenkins setup scales to handle increased load or more repositories?
Implemented Jenkins Distributed Builds by adding more Jenkins agents (slaves).
Used Kubernetes-based Jenkins setup to dynamically scale agents based on demand.
Configured master-slave setup and node labels to distribute workloads efficiently.

### 22. Have you implemented any performance optimizations in your Jenkins jobs?
Used pipeline stages to parallelize build and test tasks.
Cached dependencies and artifacts to reduce build times.
Configured incremental builds and selective testing to optimize pipeline performance.