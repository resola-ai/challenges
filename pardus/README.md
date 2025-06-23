# Code Challenge: Cloud Platform Engineering

**Objective:**

Design and implement a production-ready AWS infrastructure for one of three applications: Twenty CRM, LiteLLM Proxy, or N8N. This challenge assesses your ability to architect, deploy, and manage cloud infrastructure with modern DevOps practices and infrastructure as code.

## Application Choices

You will choose **one** of the following applications to deploy:

### Option 1: Twenty CRM
- **Application**: Open-source CRM with customer management, deal tracking, and pipeline management
- **Documentation**: [Twenty Docker Compose](https://twenty.com/developers/section/self-hosting/docker-compose)
- **Key Features**: PostgreSQL database, file storage, RESTful API, React frontend
- **Infrastructure Needs**: Container orchestration, database, file storage, load balancing

### Option 2: LiteLLM Proxy
- **Application**: Production-ready proxy server for managing multiple LLM providers
- **Documentation**: [LiteLLM Docker Deployment](https://docs.litellm.ai/docs/proxy/deploy#deploy-with-database--redis)
- **Key Features**: Multi-LLM provider support, PostgreSQL database, Redis caching, API management
- **Infrastructure Needs**: Container orchestration, database, caching, API gateway, monitoring

### Option 3: N8N
- **Application**: Workflow automation platform with visual workflow builder
- **Documentation**: [N8N Docker Installation](https://docs.n8n.io/hosting/installation/docker/)
- **Key Features**: Workflow automation, database persistence, Redis caching, worker processes
- **Infrastructure Needs**: Container orchestration, database, caching, worker scaling, queue management

## Requirements:

### Phase 1: Infrastructure Design & Architecture (2-3 days)

#### Infrastructure as Code (IaC):
- **Terraform or CloudFormation**: Complete infrastructure provisioning
- **Modular Design**: Organized, reusable components and modules
- **Environment Management**: Dev, staging, and production environments
- **State Management**: Proper Terraform state management or CloudFormation stacks
- **Variable Management**: Environment-specific configurations

#### Core Infrastructure Components:
- **VPC Design**: Multi-AZ VPC with public and private subnets
- **Networking**: Security groups, NACLs, route tables, internet/NAT gateways
- **Load Balancing**: Application Load Balancer with SSL/TLS termination
- **Auto Scaling**: Horizontal and vertical scaling policies
- **Database**: RDS PostgreSQL with multi-AZ deployment and read replicas
- **Caching**: ElastiCache Redis cluster (for LiteLLM and N8N)
- **Storage**: S3 buckets for file storage and backups
- **Monitoring**: CloudWatch, CloudTrail, and custom metrics

#### Application-Specific Infrastructure:

##### For Twenty CRM:
- **ECS Fargate**: Container orchestration for Twenty server and worker
- **RDS PostgreSQL**: Database with encryption and automated backups
- **S3**: File storage for attachments and documents
- **CloudFront**: CDN for static assets

##### For LiteLLM Proxy:
- **ECS Fargate**: Container orchestration for LiteLLM proxy
- **RDS PostgreSQL**: Database for user management and request logging
- **ElastiCache Redis**: Caching layer for responses and sessions
- **API Gateway**: API management and rate limiting
- **CloudWatch**: Request monitoring and cost tracking

##### For N8N:
- **ECS Fargate**: Container orchestration for N8N web and worker nodes
- **RDS PostgreSQL**: Database for workflow storage and execution history
- **ElastiCache Redis**: Caching and queue management
- **SQS**: Message queuing for workflow execution
- **S3**: File storage for workflow exports and imports

### Phase 2: DevOps & CI/CD Implementation (1-2 days)

#### CI/CD Pipeline:
- **GitHub Actions or AWS CodePipeline**: Automated deployment pipeline
- **Container Registry**: ECR for Docker image management
- **Security Scanning**: Container vulnerability scanning
- **Testing**: Infrastructure and application testing
- **Rollback Strategy**: Automated rollback capabilities

#### Configuration Management:
- **Secrets Management**: AWS Secrets Manager for sensitive data
- **Parameter Store**: Application configuration management
- **Environment Variables**: Secure environment variable handling
- **Configuration Validation**: Infrastructure and application config validation

#### Monitoring & Observability:
- **Logging**: Centralized logging with CloudWatch Logs
- **Metrics**: Custom CloudWatch metrics and dashboards
- **Alerting**: Automated alerting for critical issues
- **Tracing**: Distributed tracing (optional)
- **Health Checks**: Application and infrastructure health monitoring

### Phase 3: Security & Compliance (1 day)

#### Security Implementation:
- **IAM**: Least privilege access with role-based permissions
- **Encryption**: Data encryption at rest and in transit
- **Network Security**: VPC security, WAF, and DDoS protection
- **Compliance**: Security best practices and compliance frameworks
- **Backup & Recovery**: Automated backup and disaster recovery

#### Security Services Integration:
- **GuardDuty**: Threat detection and monitoring
- **Security Hub**: Security posture management
- **KMS**: Key management for encryption
- **WAF**: Web application firewall
- **Shield**: DDoS protection

## Technical Requirements:

### Infrastructure Architecture:
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Internet      │    │   CloudFront    │    │   ALB + WAF     │
│                 │───▶│   (CDN)         │───▶│                 │
│                 │    │                 │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │                       │
                                ▼                       ▼
                       ┌─────────────────┐    ┌─────────────────┐
                       │   S3 Storage    │    │   ECS Fargate   │
                       │   + Backups     │    │   (Containers)  │
                       └─────────────────┘    └─────────────────┘
                                │                       │
                                ▼                       ▼
                       ┌─────────────────┐    ┌─────────────────┐
                       │   RDS + Redis   │    │   CloudWatch    │
                       │   (Database)    │    │   + Monitoring  │
                       └─────────────────┘    └─────────────────┘
```

### Technology Stack:
- **IaC**: Terraform or CloudFormation
- **Container Orchestration**: ECS Fargate
- **Database**: RDS PostgreSQL
- **Caching**: ElastiCache Redis (for LiteLLM and N8N)
- **Storage**: S3, EFS (if needed)
- **Load Balancing**: ALB with SSL termination
- **CDN**: CloudFront
- **Monitoring**: CloudWatch, CloudTrail
- **CI/CD**: GitHub Actions or AWS CodePipeline

### Application-Specific Configurations:

#### Twenty CRM Configuration:
```yaml
# docker-compose.yml
version: '3.8'
services:
  twenty-server:
    image: twentyhq/twenty-server:latest
    environment:
      - APP_SECRET=${APP_SECRET}
      - SERVER_URL=${SERVER_URL}
      - PGPASSWORD_SUPERUSER=${PGPASSWORD_SUPERUSER}
    volumes:
      - twenty-data:/app/storage
    depends_on:
      - postgres

  twenty-worker:
    image: twentyhq/twenty-worker:latest
    environment:
      - APP_SECRET=${APP_SECRET}
      - PGPASSWORD_SUPERUSER=${PGPASSWORD_SUPERUSER}
    depends_on:
      - postgres

  postgres:
    image: postgres:15
    environment:
      - POSTGRES_PASSWORD=${PGPASSWORD_SUPERUSER}
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  twenty-data:
  postgres-data:
```

#### LiteLLM Configuration:
```yaml
# litellm-config.yaml
model_list:
  - model_name: gpt-4o
    litellm_params:
      model: azure/gpt-4o
      api_base: os.environ/AZURE_API_BASE
      api_key: os.environ/AZURE_API_KEY
      api_version: "2025-01-01-preview"

general_settings:
  master_key: os.environ/LITELLM_MASTER_KEY
  database_url: os.environ/DATABASE_URL
  redis_url: os.environ/REDIS_URL
  block_robots: true

litellm_settings:
  drop_params: true
  success_callback: ["langfuse"]
  failure_callback: ["langfuse"]
```

#### N8N Configuration:
```yaml
# docker-compose.yml
version: '3.8'
services:
  n8n:
    image: n8nio/n8n:latest
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=n8n
      - DB_POSTGRESDB_PASSWORD=${DB_PASSWORD}
      - REDIS_HOST=redis
      - REDIS_PORT=6379
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=${N8N_USER}
      - N8N_BASIC_AUTH_PASSWORD=${N8N_PASSWORD}
    volumes:
      - n8n_data:/home/node/.n8n
    depends_on:
      - postgres
      - redis

  n8n-worker:
    image: n8nio/n8n:latest
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=n8n
      - DB_POSTGRESDB_PASSWORD=${DB_PASSWORD}
      - REDIS_HOST=redis
      - REDIS_PORT=6379
      - EXECUTIONS_MODE=worker
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:15
    environment:
      - POSTGRES_DB=n8n
      - POSTGRES_USER=n8n
      - POSTGRES_PASSWORD=${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

volumes:
  n8n_data:
  postgres_data:
  redis_data:
```

## Deliverables:

### Phase 1 Deliverables:
- **Infrastructure as Code**: Complete Terraform or CloudFormation templates
- **Architecture Diagram**: Detailed infrastructure design
- **Documentation**: Infrastructure design decisions and rationale
- **Environment Setup**: Dev, staging, and production configurations

### Phase 2 Deliverables:
- **CI/CD Pipeline**: Automated deployment pipeline
- **Container Images**: ECR repositories and image management
- **Configuration Management**: Secrets and parameter management
- **Monitoring Setup**: Dashboards and alerting configuration

### Phase 3 Deliverables:
- **Security Documentation**: Security controls and compliance
- **Backup & Recovery**: Disaster recovery procedures
- **Performance Testing**: Load testing and optimization
- **Cost Optimization**: Resource optimization recommendations

## Evaluation Criteria:

### Infrastructure Design (40%):
- **Architecture Quality**: Scalable, maintainable, and secure design
- **IaC Quality**: Clean, modular, and reusable code
- **Best Practices**: AWS well-architected framework compliance
- **Documentation**: Clear and comprehensive documentation

### DevOps Implementation (30%):
- **CI/CD Pipeline**: Automated and reliable deployment process
- **Configuration Management**: Secure and efficient configuration handling
- **Monitoring**: Comprehensive observability and alerting
- **Testing**: Infrastructure and application testing

### Security & Compliance (20%):
- **Security Controls**: Proper security implementation
- **Compliance**: Security best practices and standards
- **Backup & Recovery**: Disaster recovery capabilities
- **Cost Management**: Resource optimization and cost control

### Technical Skills (10%):
- **AWS Knowledge**: Deep understanding of AWS services
- **Container Orchestration**: ECS and Docker expertise
- **Database Management**: RDS and caching expertise
- **Problem Solving**: Creative and efficient solutions

## Timeline:

This challenge is designed to be completed in **5-7 business days**:

#### **Days 1-2: Infrastructure Design**
- Choose application and design architecture
- Create infrastructure as code templates
- Set up development environment

#### **Days 3-4: Infrastructure Deployment**
- Deploy core infrastructure components
- Configure application-specific services
- Set up monitoring and logging

#### **Days 5-6: DevOps & CI/CD**
- Implement CI/CD pipeline
- Configure security controls
- Set up backup and recovery

#### **Day 7: Testing & Documentation**
- Performance testing and optimization
- Security testing and validation
- Complete documentation and final review

## Priority Focus Areas:

#### **Must Complete (High Priority):**
- Core infrastructure deployment
- Application deployment and configuration
- Basic CI/CD pipeline
- Essential security controls
- Monitoring and logging setup

#### **Should Complete (Medium Priority):**
- Multi-environment setup
- Advanced security features
- Performance optimization
- Cost optimization
- Comprehensive testing

#### **Nice to Have (Low Priority):**
- Advanced monitoring and alerting
- Disaster recovery automation
- Advanced CI/CD features
- Performance benchmarking
- Detailed cost analysis

## Questions?

Any questions you may have, please contact us by e-mail.

Good luck! 🚀
