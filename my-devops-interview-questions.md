# DevOps Interview Questions

## Git

### Explain the difference between git rebase and git merge. When would you use one over the other?
Rebase gives a cleaner, linear commit history by reapplying commits on top of a new base.
Merge preserves the full branch history by creating a new merge commit.

### How would you resolve merge conflicts in Git, and what are the best practices to avoid them?
If multiple people modify the same part of a file, Git will create a merge conflict that I need to manually resolve by editing the file and choosing the correct code.
To avoid merge conflicts, a best practice is to always pull (git pull) before starting edits, and to work in smaller branches with frequent syncing.

### What is Git's staging area and how does it differ from the working directory?
When you run git add <file>, you move the file from the working directory into the staging area, making it ready for committing.

## Jenkins

### What are Jenkins pipelines and how do declarative and scripted pipelines differ?
A Jenkins pipeline is a series of automated steps that define the process for building, testing, and deploying your software project.

- Declarative Pipeline: Structured, less flexible, Easier, more readable and maintainable, Less control, simpler for standard jobs, Standardized CI/CD pipelines
- Scripted Pipeline: Flexible, more scripting with Groovy, More complex, More control over pipeline logic, Custom, complex workflows

### How do you configure Jenkins to trigger a build automatically when code is pushed to GitHub?
To automatically trigger a Jenkins build when code is pushed to GitHub, we configure a webhook in the GitHub repository settings. The webhook is pointed to Jenkins’ URL (typically http://your-jenkins-server/github-webhook/).

In Jenkins, we configure the job to listen for GitHub events by enabling 'GitHub hook trigger for GITScm polling' or by using the GitHub Integration plugin.

### How would you implement a Jenkins pipeline to deploy a Dockerized application?
We can install the Docker Pipeline plugin to simplify Docker commands inside the pipeline. In the Jenkinsfile, we define the stages to build the Docker image, push it to a registry, and deploy the container.

## Ansible

### What are Ansible playbooks, and how do they differ from roles?
- Playbooks: These are the main units of automation in Ansible that define what should be done and on which hosts. They are more task-focused.
- Roles: Roles are a way of organizing tasks into reusable units, making it easier to share and manage automation for specific services or applications. They are more modular and structured.

### How does Ansible manage idempotency in tasks?
In Ansible, idempotency ensures that tasks are applied only once, even if the playbook is run multiple times.
Ansible will check the current state before making any changes. If the task is already in the desired state, Ansible will skip it and make no changes, ensuring that the system's state remains consistent.

### How would you use Ansible Vault to securely manage sensitive data?
Ansible Vault is a powerful tool used to securely store and manage sensitive data, such as passwords, API keys, and other credentials, within Ansible playbooks and variables.

```
ansible-vault create secrets.yml
ansible-vault encrypt myfile.yml
ansible-vault decrypt secrets.yml
ansible-vault edit secrets.yml
```

## Docker

### What is the difference between a Docker image and a Docker container?
- Docker Image: The blueprint that contains all the necessary software dependencies and instructions to run an app. Created using a Dockerfile and built with the docker build command.
- Docker Container: The running instance of a Docker image, isolated and dynamic, which executes the application defined in the image.

### How do multi-stage builds in Dockerfiles improve container optimization?
Multi-stage builds in Docker are a way to optimize Docker images by allowing you to use multiple FROM statements within a single Dockerfile.
Each stage creates a separate intermediate image, but only the final stage produces the actual image that gets deployed.

### Explain how to link Docker containers for inter-container communication.
- Docker Networks: By default, containers run in an isolated network (the bridge network)
- Bridge Network: Containers on the same bridge network can communicate with each other by using their container names as hostnames.
- Host Network: Containers share the network stack of the host machine, allowing for direct communication with the host and other containers.
- Overlay Network: It encapsulates the traffic between containers into packets that can travel over the physical network, providing secure communication even between containers on different hosts.

## Kubernetes

### What is the main difference between Deployment and StatefulSet in Kubernetes?
[Question was listed but the answer was missing.]

### Explain the role of etcd in a Kubernetes cluster.
Distributed key-value store used by Kubernetes to store cluster state and configuration data. It holds cluster data (nodes, pods, configs, secrets, etc.) and current state (used by the control plane). If etcd goes down or is corrupted, the control plane can’t function properly.

### How does Kubernetes handle container scaling?
Kubernetes supports manual and automatic scaling. Manually increases or decreases the number of pod replicas.

### How does Horizontal Pod Autoscaler (HPA) work in Kubernetes?
HPA automatically adjusts the number of pod replicas based on resource usage such as CPU utilization, memory usage, or custom metrics via Prometheus or external metrics API. Metrics are collected by the metrics-server. HPA compares current usage to the target threshold and scales pods accordingly. Runs in a loop (default every 15 seconds).

## SonarQube

### How does SonarQube integrate with Jenkins for code analysis?
Install the SonarQube plugin in Jenkins. Configure SonarQube in Jenkins and add the connection details to your SonarQube server (e.g., URL, authentication token). Add a SonarQube analysis step in your build configuration.

### What metrics does SonarQube use to assess code quality, and what is the significance of each?
- Bugs
- Vulnerabilities
- Duplications

### How would you configure SonarQube to enforce Quality Gates before deployment?
A Quality Gate in SonarQube is a set of conditions that a project must meet before it can be considered as "good quality." It’s used to enforce code quality standards before code is deployed.

## Terraform

### How does Terraform handle the state file and why is the Terraform state file important?
Essential for tracking infrastructure state and avoiding redundant changes. Remote backends are recommended for collaboration, locking, and security.

### What are Terraform modules, and how do they help in organizing Infrastructure as Code?
Help in reusing and organizing your infrastructure code. Modules allow you to define, organize, and maintain reusable components of infrastructure.

### How do Terraform workspaces help when deploying across multiple environments like dev, staging, and prod?
Help in managing separate environments (dev, staging, prod) by isolating state for each workspace. They provide a clean separation of environments and make managing multi-environment deployments easier.

## Monitoring (Prometheus & Grafana)

### How do you set up Prometheus to scrape metrics from an application?
Expose an HTTP endpoint. In prometheus.yml, specify the targets (i.e., HTTP URL).

### Explain how Grafana queries data from Prometheus.
Add Prometheus as a Data Source in Grafana. In the URL field, enter the address of your Prometheus server. In the Query editor, you can write Prometheus PromQL queries to retrieve the metrics you want.

### What are alerting rules in Prometheus and how do you integrate them with Grafana for notifications?
Alerting rules are defined in the prometheus.yml or in a separate alert_rules.yml. Configure Alertmanager in prometheus.yml to receive the alerts. Integrate Alertmanager with Grafana. Alertmanager can handle alerts and send them to various notification channels (email, Slack, etc.).