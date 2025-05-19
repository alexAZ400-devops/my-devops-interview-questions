
# DevOps Interview Questions

1. **What is the difference between DaemonSet and StatefulSet?**  
DaemonSet: It ensures that the pod is running in the node. If the pod is down, it always ensures one pod per node is running.  
StatefulSet: Manages stateful applications, like databases, backend part which maintains its state even after session ends.

2. **Can you write a YAML file for a Kubernetes deployment?**  
Basic syntax
```yaml
apiVersion: v2
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 4
  selector:
  template:
```

3. **What is the difference between PersistentVolume (PV) and PersistentVolumeClaim (PVC)?**  
PV is the actual storage provisioned assigned by Admin and can be dynamically or manually assigned.  
PVC is the request to use the PV resource created by user and it needs a matching PV to bind.

4. **PVC is not bound—how would you troubleshoot the issue?**  
- `kubectl get pv`  
- Ensure PV matches PVC requirements (storage, access mode)  
- `kubectl describe pvc my-pvc`  
- `kubectl get storageclass`  
- Ensure volume is not already bound.

5. **What is the difference between kubectl apply and kubectl create?**  
- kubectl create → Only creates a resource; fails if it already exists.
- kubectl apply → Creates the resource if it doesn't exist or updates it if it does.

6. **What is blue-green deployment?**  
Blue is current version. Green is new version. After testing and checking traffic will be shifted from Blue to Green.

7. **Can you explain Docker volumes?**  
- Persist data beyond container lifecycle.  
- Share data between containers.  
- Avoid data loss on container restarts.

8. **What command do you use to check network statistics in Linux?**  
`netstat`

9. **What is the command to download a package in Linux?**  
`apt-get install python3`

10. **What is the command for Maven compile?**  
`mvn compile`

11. **How do you delete a Docker image?**  
`docker rmi imagename`

12. **How do you list all Docker containers including stopped ones?**  
`docker ps -a`

13. **Can you write a Jenkins pipeline syntax?**  
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
```

14. **What stages do you define in a Jenkins pipeline?**  
Build, test, deploy.

15. **How do you integrate SonarQube with Jenkins?**  
- Install SonarQube Scanner plugin in Jenkins.  
- Configure SonarQube server details in Jenkins.  
- Use a Jenkins pipeline to run the SonarQube analysis.

16. **Where do you configure the SonarQube server in Jenkins?**  
Manage Jenkins → Configure System → Add SonarQube details.

17. **What details are needed to configure the SonarQube server in Jenkins?**  
Server URL, Authentication Token, SonarQube Scanner path, Project key for analysis

18. **What is a SonarQube quality gate?**  
Set of defined conditions to assess whether a project passes or fails SonarQube analysis based on metrics like code coverage, security vulnerabilities, and duplicated code.

19. **Can you explain AWS VPC and its components?**  
VPC is logically isolated network in AWS, allowing you to control IP ranges, subnets, and routing. Key components include:  
Subnets (Public & Private), Internet Gateway (IGW), NAT Gateway, Route Tables, Security Groups, Network ACLs, VPC Peering & Transit Gateway

20. **How many internet gateways can you have for a VPC by default?**  
One IGW per VPC

21. **Can you create additional internet gateways for a VPC?**  
No

22. **In which subnet should the NAT gateway be created, and why?**  
It should be in a public subnet with an Elastic IP (EIP), so it can route traffic from private subnets to the internet.

23. **What configurations are needed to enable communication from private subnets using NAT Gateway?**  
- Deploy NAT Gateway in a public subnet.  
- Associate it with an Elastic IP.  
- Modify route tables for private subnets to forward traffic through NAT Gateway.

24. **What is the use of Route 53?**  
AWS’s DNS service for domain registration, routing traffic, and health checks.

25. **What are hosted zones in Route 53?**  
- Public Hosted Zone: Routes traffic to the internet.  
- Private Hosted Zone: Routes traffic within a VPC.

26. **What are S3 Standard and S3 Glacier? What is the difference?**  
- S3 Standard: High-performance object storage for frequently accessed data.  
- S3 Glacier: Cost-effective cold storage for long-term archival.

27. **What are EC2 Spot Instances?**  
Discounted EC2 instances suitable for workloads that can tolerate interruptions.

28. **How do you enable communication between two VPCs?**  
Use VPC Peering, Transit Gateway (connect multiple VPCs), VPN Connection (site-to-site), PrivateLink (service-based connection)

29. **What are the types of load balancers in AWS?**  
- Application Load Balancer (ALB)  
- Network Load Balancer (NLB)  
- Gateway Load Balancer (GLB)

30. **What is an Application Load Balancer (ALB)?**  
Works at Layer 7 (HTTP/HTTPS), supports routing based on paths, host headers, and advanced traffic management.

31. **What components fall under layer 7?**  
HTTP, HTTPS, DNS, FTP, SMTP, API communication

32. **What is the difference between mvn install and mvn package?**  
- `mvn package`: Compiles source code, runs tests, and creates a JAR/WAR file inside the target/ directory.  
- `mvn install`: Does everything `mvn package` does, but also stores the built artifact in the local Maven repository (`~/.m2/repository`) for dependency management.

33. **How do you scan a Docker image for vulnerabilities?**  
Use Trivy (lightweight vulnerability scanner): `trivy image <image-name>`

34. **How do you get the scan report using Trivy?**  
Generate a detailed JSON or table report: `trivy image --format json -o report.json <image-name>`
