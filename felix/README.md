# Code Challenge: AWS Application Security & Penetration Testing

**Objective:**

Deploy Twenty CRM using Docker Compose in AWS and implement essential security controls to protect it. Then conduct focused penetration testing to identify and document security vulnerabilities. This challenge assesses your ability to secure cloud applications and perform security assessments within a realistic timeframe.

## Sample Application: Twenty CRM

You will deploy and secure **Twenty CRM** - an open-source CRM application built with modern web technologies. This application handles sensitive customer data, business information, and user credentials, requiring robust security measures.

### Application Features:
- **Customer Relationship Management**: Contact management, deal tracking, and pipeline management
- **User Authentication & Authorization**: Multi-user support with role-based access
- **Database Persistence**: PostgreSQL database for all CRM data
- **File Storage**: Document and attachment management
- **API Endpoints**: RESTful API for integrations and automation
- **Web Interface**: Modern React-based frontend

## Requirements:

### Phase 1: Twenty CRM Deployment & Security Implementation (2-3 days)

#### Essential AWS Infrastructure Security:
- **Secure VPC Design**: Private subnets for database, public subnets with NAT gateways
- **Network Security**: Security groups with minimal required access
- **Load Balancer**: Application Load Balancer with SSL/TLS termination
- **Basic Monitoring**: CloudWatch for essential logging

#### Twenty CRM Configuration & Security:
- **Docker Compose Setup**: Deploy using Twenty's official Docker setup
- **Database Security**: PostgreSQL with encryption at rest
- **Environment Variables**: Secure configuration management
- **Authentication**: Basic JWT-based authentication
- **Input Validation**: Essential input sanitization
- **Secure Headers**: Basic security headers (HSTS, X-Frame-Options)

#### AWS Security Services Integration:
- **IAM**: Basic least privilege access for Twenty CRM components
- **KMS**: Key management for database encryption
- **Secrets Manager**: Secure storage of database credentials
- **WAF**: Basic web application firewall rules

### Phase 2: Penetration Testing & Security Assessment (1-2 days)

#### Focused Penetration Testing Scope:
- **Twenty CRM Web Application Testing**: Core authentication and data access endpoints
- **Database Security Testing**: PostgreSQL configuration and access
- **Basic Infrastructure Testing**: AWS service configuration and network security
- **Authentication Testing**: User authentication and session management

#### Penetration Testing Tools (Choose 2-3):
- **Burp Suite**: Web application security testing
- **OWASP ZAP**: Open-source web application scanner
- **Nmap**: Network discovery and port scanning
- **AWS CLI**: Basic AWS service enumeration

#### Required Testing Areas:

##### Twenty CRM Web Application Testing:
- **Authentication Bypass**: Test for ways to access endpoints without proper authentication
- **Authorization Testing**: Verify user access controls and admin privileges
- **Input Validation**: Test for basic injection attacks in forms
- **Session Management**: Test session security and token handling

##### Database & Infrastructure Security Testing:
- **Database Enumeration**: Identify exposed database endpoints
- **Data Exposure**: Check for sensitive data in responses or logs
- **Connection Security**: Verify encrypted connections to database
- **Basic Network Security**: Test security groups and access controls

### Technical Requirements:

#### Deployment Architecture:
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Internet      │    │   ALB + WAF     │    │   Twenty CRM    │
│                 │───▶│                 │───▶│   (Docker)      │
│                 │    │                 │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │                       │
                                ▼                       ▼
                       ┌─────────────────┐    ┌─────────────────┐
                       │   CloudWatch    │    │   PostgreSQL    │
                       │   Logging       │    │   Database      │
                       └─────────────────┘    └─────────────────┘
```

#### Technology Stack:
- **Application**: Twenty CRM (Docker Compose)
- **Database**: PostgreSQL with encryption
- **Infrastructure**: ECS with Fargate or EC2 instances
- **Security**: WAF, KMS, Secrets Manager
- **Monitoring**: CloudWatch

#### Twenty CRM Configuration Example:
Based on the [Twenty Docker Compose documentation](https://twenty.com/developers/section/self-hosting/docker-compose):

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

```ini
# .env file
APP_SECRET=your_generated_secret_here
SERVER_URL=https://your-domain.com
PGPASSWORD_SUPERUSER=your_strong_password
```

### Deliverables:

#### Phase 1 Deliverables:
- **Infrastructure as Code**: Basic Terraform or CloudFormation templates
- **Twenty CRM Configuration**: Secure docker-compose.yml and .env files
- **Database Setup**: PostgreSQL with basic security
- **Security Documentation**: Essential security controls documentation
- **Architecture Diagram**: Simple visual representation
- **Deployment Guide**: Basic setup instructions

#### Phase 2 Deliverables:
- **Penetration Testing Report**: Focused security assessment (max 10 pages)
- **Vulnerability Documentation**: Key findings with CVSS scores
- **Remediation Recommendations**: Top 5 critical fixes
- **Testing Methodology**: Brief description of tools and approach

### Penetration Testing Report Structure:

#### Executive Summary (1 page):
- **Scope**: What was tested
- **Risk Assessment**: Overall security posture
- **Critical Findings**: Top 3-5 vulnerabilities
- **Key Recommendations**: Essential remediation steps

#### Technical Findings (5-7 pages):
- **Vulnerability Details**: Description, impact, and likelihood
- **Proof of Concept**: Evidence and reproduction steps
- **CVSS Scores**: Standardized risk assessment
- **Remediation**: Specific fix recommendations

#### Methodology (1-2 pages):
- **Tools Used**: Penetration testing tools and techniques
- **Testing Approach**: Brief methodology and scope
- **Limitations**: What couldn't be tested

### Evaluation Criteria:

#### Security Implementation (50%):
- **Infrastructure Security**: Essential AWS security controls
- **Twenty CRM Security**: Secure configuration and deployment
- **Database Security**: Basic encryption and access controls
- **Documentation**: Clear and concise documentation

#### Penetration Testing (50%):
- **Testing Methodology**: Efficient and focused approach
- **Finding Quality**: Relevance and accuracy of vulnerabilities
- **Report Quality**: Clear, actionable findings
- **Remediation**: Practical and effective recommendations

### Timeline:

This challenge is designed to be completed in **3-5 business days**:

#### **Days 1-2: Infrastructure & Deployment**
- Set up AWS infrastructure
- Deploy Twenty CRM using Docker Compose
- Implement basic security controls

#### **Days 3-4: Security Hardening**
- Configure WAF and security groups
- Set up monitoring and logging
- Test basic functionality

#### **Day 5: Penetration Testing & Report**
- Conduct focused penetration testing
- Document findings and recommendations
- Complete final report

### Priority Focus Areas:

#### **Must Complete (High Priority):**
- Basic AWS infrastructure security
- Twenty CRM deployment and configuration
- Database encryption and access controls
- Authentication and authorization testing
- Input validation testing

#### **Should Complete (Medium Priority):**
- WAF configuration
- Basic monitoring setup
- Session management testing
- Error handling assessment

#### **Nice to Have (Low Priority):**
- Advanced monitoring and alerting
- Comprehensive security testing
- Detailed documentation
- Performance optimization

### Sample Vulnerabilities to Look For:

#### High Priority (Focus Here):
- Database connection string exposure
- Authentication bypass in CRM
- Sensitive data exposure in responses
- SQL injection in search/filter functions
- Missing security headers

#### Medium Priority:
- Cross-site scripting (XSS) in forms
- Rate limiting bypass
- Information disclosure in error messages
- Weak session management

#### Low Priority:
- Verbose error messages
- Outdated dependencies
- Unnecessary open ports

### Twenty CRM-Specific Security Considerations:

#### Essential Docker Security:
- **Container Configuration**: Secure environment variables
- **Network Security**: Basic container network isolation
- **Resource Limits**: CPU and memory limits

#### Essential Database Security:
- **Encryption**: PostgreSQL encryption at rest
- **Access Control**: Basic database user permissions
- **Connection Security**: SSL/TLS for database connections

#### Essential Web Application Security:
- **Input Validation**: Basic user input validation
- **Session Security**: Secure session management
- **API Security**: Secure RESTful API endpoints

## Questions?

Any questions you may have, please contact us by e-mail.

Good luck! 🚀 