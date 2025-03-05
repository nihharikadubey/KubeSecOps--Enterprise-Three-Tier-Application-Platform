# KubeSecOps: Enterprise Three-Tier Application Platform

> A Production-Ready DevSecOps Implementation with Kubernetes, AWS, and Automated Security Controls

## Project Description

KubeSecOps is a comprehensive, enterprise-grade DevSecOps platform that demonstrates the implementation of a secure, scalable, and highly available three-tier application architecture. Built on AWS EKS with a focus on security and automation, this project showcases industry best practices for deploying and managing containerized applications in production environments.

### Key Features

- **Complete CI/CD Pipeline**: Automated build, test, and deployment workflows using Jenkins
- **Security-First Approach**: Integrated security scanning, SAST/DAST testing, and compliance checks
- **Infrastructure as Code**: Fully automated AWS infrastructure provisioning using Terraform
- **Container Orchestration**: Production-ready Kubernetes configurations with auto-scaling
- **Monitoring & Observability**: Comprehensive monitoring stack with Prometheus and Grafana
- **Database Management**: Automated backup and recovery procedures for MongoDB
- **Cost Optimization**: Resource optimization and cloud cost management strategies

### Target Audience

- DevOps Engineers
- Cloud Architects
- Security Engineers
- Development Teams
- Infrastructure Teams
- SRE Practitioners

## Project Overview
This project implements a comprehensive DevSecOps pipeline for a Three-Tier application deployment on AWS EKS. The application consists of a React frontend for user interaction, Node.js backend for business logic, and MongoDB for data persistence. The entire stack is containerized, security-scanned, and deployed using modern DevSecOps practices.

## Architecture
```
                                     AWS Cloud
┌────────────────────────────────────────────────────────────────────┐
│                                                                    │
│   ┌─────────────┐    ┌─────────────┐    ┌─────────────┐          │
│   │   AWS ALB   │    │   AWS EKS   │    │  AWS ECR    │          │
│   │  (Ingress)  │    │  Cluster    │    │ Repositories│          │
│   └──────┬──────┘    └──────┬──────┘    └─────────────┘          │
│          │                   │                                     │
│          │            ┌──────┴──────┐                             │
│          │            │   K8s       │                             │
│          │            │  Services   │                             │
│          │            └──────┬──────┘                            │
│          │                   │                                    │
│    ┌─────┴─────┐      ┌─────┴─────┐     ┌─────────────┐         │
│    │  Frontend │      │  Backend  │     │  MongoDB    │         │
│    │  (React)  │─────▶│ (Node.js) │────▶│  Database   │         │
│    └───────────┘      └───────────┘     └─────────────┘         │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
```

## Project Workflow
```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Developer     │     │    Jenkins      │     │     AWS         │
│   Commits Code  │────▶│  CI/CD Pipeline │────▶│  Infrastructure │
└─────────────────┘     └─────────────────┘     └─────────────────┘
         │                       │                       │
         │                       │                       │
         │                       ▼                       ▼
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   SonarQube     │     │     Docker      │     │      ECR        │
│  Code Analysis  │◀────│     Build       │────▶│  Image Registry │
└─────────────────┘     └─────────────────┘     └─────────────────┘
         │                       │                       │
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│     Trivy       │     │     OWASP       │     │      EKS        │
│ Security Scan   │     │ Dependency Check│     │    Deployment   │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```
![Project workflow](\assets\Three-Tier.gif)
## Detailed Technology Stack

### Infrastructure & Platform
- **AWS Services**
  - EKS (Elastic Kubernetes Service) for container orchestration
  - ECR (Elastic Container Registry) for Docker images
  - ALB (Application Load Balancer) for traffic distribution
  - IAM for access management
  - VPC for network isolation
  - EC2 for Jenkins server

- **Container Orchestration**
  - Kubernetes 1.21+
  - eksctl for cluster management
  - kubectl for cluster interaction
  - Helm 3.x for package management

- **Infrastructure as Code**
  - Terraform 1.0+
  - AWS Provider
  - Local Provider

### CI/CD & DevSecOps Tools
- **Jenkins**
  - Version: 2.375.2 LTS
  - Plugins:
    - Docker Pipeline
    - Kubernetes CLI
    - AWS Credentials
    - SonarQube Scanner
    - OWASP Dependency-Check
    - Blue Ocean

- **Security Tools**
  - SonarQube 9.x
    - Quality Gates
    - Code Coverage
    - Vulnerability Scanning
  - Trivy
    - Container Scanning
    - Filesystem Scanning
    - Configuration Scanning
  - OWASP Dependency Check
    - NPM Package Scanning
    - Third-party Library Analysis

- **Monitoring Stack**
  - Prometheus
    - Node Exporter
    - Alert Manager
    - Push Gateway
  - Grafana
    - Custom Dashboards
    - Alert Configuration
    - Metrics Visualization

### Application Components

#### Frontend (React.js)
- **Core Technologies**
  - React 17.x
  - Material-UI 5.x
  - Axios for API calls
  - React Router for navigation

- **Features**
  - Responsive design
  - Dark/Light theme
  - Error handling
  - Loading states
  - Form validation

- **Docker Configuration**
```dockerfile
FROM node:14-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

#### Backend (Node.js)
- **Core Technologies**
  - Node.js 14.x
  - Express 4.x
  - Mongoose ODM
  - JWT for authentication
  - Winston for logging

- **API Endpoints**
  - `/api/v1/tasks` - CRUD operations
  - `/api/v1/auth` - Authentication
  - `/health` - Health checks
  - `/metrics` - Prometheus metrics

- **Docker Configuration**
```dockerfile
FROM node:14-alpine
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["node", "server.js"]
```

#### Database (MongoDB)
- **Configuration**
  - Version: 5.0
  - Authentication: SCRAM-SHA-256
  - Storage Engine: WiredTiger
  - Replica Set: 1 primary (for development)

- **Data Models**
  - Tasks
  - Users
  - Audit Logs

## Infrastructure Setup (Detailed)

### 1. Jenkins Server Deployment

#### Prerequisites
```bash
# Install required AWS CLI version
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Install Terraform
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
```

#### Terraform Configuration
```hcl
# VPC Configuration
resource "aws_vpc" "jenkins_vpc" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true

  tags = {
    Name = "jenkins-vpc"
  }
}

# Security Group
resource "aws_security_group" "jenkins_sg" {
  name        = "jenkins-sg"
  description = "Security group for Jenkins server"
  vpc_id      = aws_vpc.jenkins_vpc.id

  ingress {
    from_port   = 8080
    to_port     = 8080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  # Additional rules...
}

# EC2 Instance
resource "aws_instance" "jenkins_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.2xlarge"
  key_name      = var.key_name

  # Additional configuration...
}
```

### 2. EKS Cluster Setup

#### Cluster Creation
```bash
# Create cluster
eksctl create cluster \
  --name three-tier-cluster \
  --region us-west-2 \
  --node-type t2.medium \
  --nodes-min 2 \
  --nodes-max 4 \
  --zones us-west-2a,us-west-2b \
  --with-oidc \
  --ssh-access \
  --ssh-public-key my-key \
  --managed

# Configure kubectl
aws eks update-kubeconfig --region us-west-2 --name three-tier-cluster

# Verify nodes
kubectl get nodes -o wide
```

#### Node Group Configuration
```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: three-tier-cluster
  region: us-west-2

nodeGroups:
  - name: ng-1
    instanceType: t2.medium
    desiredCapacity: 2
    minSize: 2
    maxSize: 4
    labels:
      role: worker
    tags:
      k8s.io/cluster-autoscaler/enabled: "true"
```

## CI/CD Pipeline Configuration

### Jenkins Pipeline Structure

#### Frontend Pipeline Details
```groovy
// Pipeline stages with detailed explanations
stage('Sonarqube Analysis') {
    steps {
        dir('Application-Code/frontend') {
            withSonarQubeEnv('sonar-server') {
                sh ''' $SCANNER_HOME/bin/sonar-scanner \
                -Dsonar.projectName=three-tier-frontend \
                -Dsonar.projectKey=three-tier-frontend '''
            }
        }
    }
}
```

Key Features:
- Automated workspace cleanup
- Source code analysis with SonarQube
- Quality gate checks with configurable thresholds
- OWASP dependency scanning for vulnerabilities
- Trivy scanning for both filesystem and container images
- Automated ECR image tagging and pushing
- GitOps-based deployment updates

#### Backend Pipeline Details
```groovy
stage("ECR Image Pushing") {
    steps {
        script {
            sh 'aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${REPOSITORY_URI}'
            sh 'docker tag ${AWS_ECR_REPO_NAME} ${REPOSITORY_URI}${AWS_ECR_REPO_NAME}:${BUILD_NUMBER}'
            sh 'docker push ${REPOSITORY_URI}${AWS_ECR_REPO_NAME}:${BUILD_NUMBER}'
        }
    }
}
```

### Quality Gates Configuration

#### SonarQube Quality Gates
```json
{
  "qualityGates": {
    "name": "Three-Tier-App Gate",
    "conditions": [
      {
        "metric": "new_reliability_rating",
        "op": "GT",
        "error": "1"
      },
      {
        "metric": "new_security_rating",
        "op": "GT",
        "error": "1"
      },
      {
        "metric": "new_maintainability_rating",
        "op": "GT",
        "error": "1"
      }
    ]
  }
}
```

## Kubernetes Deployment Manifests

### Frontend Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  namespace: three-tier-app
spec:
  replicas: 2
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  template:
    spec:
      containers:
      - name: frontend
        image: ${ECR_REPO}/frontend:latest
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 3000
        readinessProbe:
          httpGet:
            path: /ready
            port: 3000
```

### Backend Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
  namespace: three-tier-app
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: backend
        image: ${ECR_REPO}/backend:latest
        env:
        - name: MONGODB_URI
          valueFrom:
            secretKeyRef:
              name: mongodb-secret
              key: uri
        resources:
          requests:
            memory: "128Mi"
            cpu: "250m"
          limits:
            memory: "256Mi"
            cpu: "500m"
```

### Database StatefulSet
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mongodb
  namespace: three-tier-app
spec:
  serviceName: mongodb
  replicas: 1
  template:
    spec:
      containers:
      - name: mongodb
        image: mongo:5.0
        volumeMounts:
        - name: mongodb-data
          mountPath: /data/db
  volumeClaimTemplates:
  - metadata:
      name: mongodb-data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```

## Monitoring Setup

### Prometheus Configuration

#### prometheus-values.yaml
```yaml
server:
  retention: 15d
  persistentVolume:
    size: 10Gi

alertmanager:
  enabled: true
  config:
    global:
      resolve_timeout: 5m
    route:
      group_wait: 30s
      group_interval: 5m
      repeat_interval: 12h
      receiver: 'slack'
      routes:
      - match:
          severity: critical
        receiver: 'slack'
```

### Grafana Dashboards

#### Application Dashboard
```json
{
  "dashboard": {
    "panels": [
      {
        "title": "Request Rate",
        "type": "graph",
        "datasource": "Prometheus",
        "targets": [
          {
            "expr": "rate(http_requests_total[5m])",
            "legendFormat": "{{method}} {{path}}"
          }
        ]
      },
      {
        "title": "Error Rate",
        "type": "graph",
        "targets": [
          {
            "expr": "rate(http_requests_errors_total[5m])",
            "legendFormat": "{{status_code}}"
          }
        ]
      }
    ]
  }
}
```

## Security Configurations

### Network Policies
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: backend-policy
  namespace: three-tier-app
spec:
  podSelector:
    matchLabels:
      app: backend
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
    ports:
    - protocol: TCP
      port: 5000
  egress:
  - to:
    - podSelector:
        matchLabels:
          app: mongodb
    ports:
    - protocol: TCP
      port: 27017
```

### Pod Security Policies
```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: restricted
spec:
  privileged: false
  allowPrivilegeEscalation: false
  requiredDropCapabilities:
    - ALL
  volumes:
    - 'configMap'
    - 'emptyDir'
    - 'projected'
    - 'secret'
    - 'downwardAPI'
    - 'persistentVolumeClaim'
  hostNetwork: false
  hostIPC: false
  hostPID: false
  runAsUser:
    rule: 'MustRunAsNonRoot'
  seLinux:
    rule: 'RunAsAny'
  supplementalGroups:
    rule: 'MustRunAs'
    ranges:
      - min: 1
        max: 65535
  fsGroup:
    rule: 'MustRunAs'
    ranges:
      - min: 1
        max: 65535
```

## Detailed Deployment Steps

### 1. Infrastructure Setup
```bash
# Clone repository
git clone https://github.com/your-repo/three-tier-app.git
cd three-tier-app

# Deploy Jenkins server
cd Jenkins-Server-TF
terraform init
terraform plan
terraform apply

# Configure Jenkins
echo "Visit http://<jenkins-ip>:8080"
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

### 2. EKS Cluster Configuration
```bash
# Create EKS cluster
eksctl create cluster -f cluster-config.yaml

# Configure kubectl
aws eks update-kubeconfig --region us-west-2 --name three-tier-cluster

# Verify cluster
kubectl get nodes
kubectl get pods -A
```

### 3. Application Deployment
```bash
# Create namespace
kubectl create namespace three-tier-app

# Apply secrets
kubectl apply -f secrets/

# Deploy MongoDB
kubectl apply -f mongodb/

# Deploy Backend
kubectl apply -f backend/

# Deploy Frontend
kubectl apply -f frontend/

# Verify deployments
kubectl get all -n three-tier-app
```

### 4. Monitoring Setup
```bash
# Add Helm repositories
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

# Install Prometheus
helm install prometheus prometheus-community/prometheus \
  --namespace monitoring \
  --create-namespace \
  -f prometheus-values.yaml

# Install Grafana
helm install grafana grafana/grafana \
  --namespace monitoring \
  --set persistence.enabled=true \
  --set adminPassword='your-secure-password'
```

### 5. Security Implementation
```bash
# Apply network policies
kubectl apply -f network-policies/

# Configure RBAC
kubectl apply -f rbac/

# Apply pod security policies
kubectl apply -f pod-security-policies/
```

## Maintenance and Troubleshooting

### Common Issues and Solutions

1. **Jenkins Pipeline Failures**
   ```bash
   # Check Jenkins logs
   sudo tail -f /var/log/jenkins/jenkins.log
   
   # Verify Jenkins plugins
   java -jar jenkins-cli.jar -s http://localhost:8080/ list-plugins
   ```

2. **EKS Cluster Issues**
   ```bash
   # Check cluster status
   eksctl get cluster
   
   # View cluster logs
   kubectl logs -n kube-system aws-node-xxxxx
   ```

3. **Application Issues**
   ```bash
   # Check pod status
   kubectl get pods -n three-tier-app
   
   # View application logs
   kubectl logs -f deployment/frontend -n three-tier-app
   kubectl logs -f deployment/backend -n three-tier-app
   ```

## Backup and Disaster Recovery

### Database Backup Strategy

#### Automated Backup Configuration
```yaml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: mongodb-backup
  namespace: three-tier-app
spec:
  schedule: "0 2 * * *"  # Daily at 2 AM
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: mongodb-backup
            image: mongo:5.0
            command:
            - /bin/sh
            - -c
            - |
              mongodump --uri="${MONGODB_URI}" --archive=/backup/db-$(date +%Y%m%d).gz --gzip
            volumeMounts:
            - name: backup-volume
              mountPath: /backup
          volumes:
          - name: backup-volume
            persistentVolumeClaim:
              claimName: mongodb-backup-pvc
```

#### Backup Retention Policy
- Daily backups: 7 days retention
- Weekly backups: 4 weeks retention
- Monthly backups: 12 months retention

### Disaster Recovery Procedures

#### EKS Cluster Recovery
```bash
# 1. Backup etcd
kubectl exec -it etcd-pod -n kube-system -- etcdctl snapshot save /backup/etcd-snapshot.db

# 2. Create new cluster with same configuration
eksctl create cluster -f cluster-config.yaml

# 3. Restore applications
kubectl apply -f manifests/

# 4. Verify recovery
kubectl get all -n three-tier-app
```

#### Application State Recovery
```bash
# 1. Restore MongoDB data
mongorestore --uri="${MONGODB_URI}" --archive=/backup/db-latest.gz --gzip

# 2. Verify data integrity
mongo "${MONGODB_URI}" --eval "db.runCommand({dbStats: 1})"

# 3. Restart applications
kubectl rollout restart deployment/backend deployment/frontend -n three-tier-app
```

## Scaling Strategies

### Horizontal Pod Autoscaling

#### Frontend HPA
```yaml
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: frontend-hpa
  namespace: three-tier-app
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: frontend
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
```

#### Backend HPA
```yaml
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: backend-hpa
  namespace: three-tier-app
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: backend
  minReplicas: 3
  maxReplicas: 15
  behavior:
    scaleUp:
      stabilizationWindowSeconds: 60
    scaleDown:
      stabilizationWindowSeconds: 300
```

### Cluster Autoscaling

#### Node Group Autoscaling
```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: three-tier-cluster
  region: us-west-2
nodeGroups:
  - name: ng-1
    instanceType: t2.medium
    minSize: 2
    maxSize: 8
    desiredCapacity: 2
    labels:
      role: worker
    tags:
      k8s.io/cluster-autoscaler/enabled: "true"
      k8s.io/cluster-autoscaler/three-tier-cluster: "owned"
```

## Cost Optimization Guidelines

### Resource Requests and Limits

#### Development Environment
```yaml
resources:
  requests:
    cpu: 100m
    memory: 128Mi
  limits:
    cpu: 200m
    memory: 256Mi
```

#### Production Environment
```yaml
resources:
  requests:
    cpu: 250m
    memory: 512Mi
  limits:
    cpu: 500m
    memory: 1Gi
```

### AWS Cost Management

#### EC2 Instance Optimization
```hcl
resource "aws_instance" "jenkins_server" {
  instance_type = var.environment == "prod" ? "t2.2xlarge" : "t2.large"
  
  root_block_device {
    volume_size = 30
    volume_type = "gp3"
  }

  tags = {
    Environment = var.environment
    CostCenter = "devops"
  }
}
```

#### EKS Cost Reduction Strategies
1. Spot Instances Configuration
```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: three-tier-cluster
nodeGroups:
  - name: spot-ng
    instanceType: mixed
    instanceTypes: ["t2.medium", "t3.medium"]
    spot: true
    minSize: 2
    maxSize: 5
```

## Performance Tuning

### Node.js Backend Optimization

#### PM2 Configuration
```json
{
  "apps": [{
    "name": "backend-api",
    "script": "server.js",
    "instances": "max",
    "exec_mode": "cluster",
    "env": {
      "NODE_ENV": "production",
      "NODE_OPTIONS": "--max-old-space-size=4096"
    }
  }]
}
```

### MongoDB Performance

#### Index Optimization
```javascript
// Create compound indexes for frequent queries
db.tasks.createIndex({ "userId": 1, "createdAt": -1 });
db.tasks.createIndex({ "status": 1, "dueDate": 1 });

// Text search index
db.tasks.createIndex({ "title": "text", "description": "text" });
```

### React Frontend Optimization

#### Production Build Configuration
```javascript
// webpack.config.js
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all',
      minSize: 20000,
      maxSize: 244000,
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all',
        },
      },
    },
    runtimeChunk: 'single',
  },
}
```

## Additional Security Hardening

### Container Security

#### Pod Security Context
```yaml
securityContext:
  runAsUser: 1000
  runAsGroup: 3000
  fsGroup: 2000
  runAsNonRoot: true
  seccompProfile:
    type: RuntimeDefault
  capabilities:
    drop:
      - ALL
```

### Network Security

#### Service Mesh Configuration (Istio)
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: frontend-vs
spec:
  hosts:
  - "frontend.example.com"
  gateways:
  - frontend-gateway
  http:
  - match:
    - uri:
        prefix: /api
    route:
    - destination:
        host: backend-svc
        port:
          number: 5000
```

### Secrets Management

#### AWS Secrets Manager Integration
```yaml
apiVersion: secrets-store.csi.x-k8s.io/v1
kind: SecretProviderClass
metadata:
  name: aws-secrets
spec:
  provider: aws
  parameters:
    objects: |
      - objectName: "prod/three-tier-app"
        objectType: "secretsmanager"
        jmesPath: 
          - path: MONGODB_URI
            objectAlias: mongodb_uri
```