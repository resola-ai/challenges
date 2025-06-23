# Code Challenge: Audit Log API

## Objective

Develop a comprehensive audit logging API system that tracks and manages user actions across different applications. This system should be designed to handle high-volume logging, provide search and filtering capabilities, and ensure data integrity and security.

## Requirements:

### Core Features:

#### Audit Log Management:
- **Log Entry Creation**: API endpoints to create audit log entries with metadata
- **Structured Data**: Each log entry should include:
  - User ID and session information
  - Action performed (CREATE, UPDATE, DELETE, VIEW, etc.)
  - Resource type and ID (e.g., "user", "order", "product")
  - Timestamp with timezone
  - IP address and user agent
  - Before/after state changes (for modifications)
  - Custom metadata fields
  - Severity level (INFO, WARNING, ERROR, CRITICAL)

#### Search and Retrieval:
- **Advanced Search**: Filter logs by date range, user, action type, resource type, severity
- **Full-text Search**: Search through log messages and metadata
- **Pagination**: Handle large result sets efficiently
- **Export Functionality**: Export logs in JSON, CSV formats
- **Real-time Log Streaming**: WebSocket endpoint for real-time log monitoring

#### Data Management:
- **Data Retention**: Configurable retention policies (e.g., keep logs for 90 days)
- **Data Archival**: Move old logs to cold storage
- **Data Compression**: Efficient storage for large log volumes
- **Backup and Recovery**: Automated backup procedures

### Technical Requirements:

#### API Design:
- **RESTful API** following OpenAPI 3.0 specification
- **Authentication**: JWT-based authentication
- **Authorization**: Role-based access (Admin, Auditor, User)
- **Rate Limiting**: Prevent API abuse
- **Request Validation**: Input sanitization and validation
- **Error Handling**: Proper HTTP status codes and error messages

#### Database Design:
- **Optimized Schema**: Design for high-volume writes and complex queries
- **Indexing Strategy**: Efficient indexes for search operations
- **Data Partitioning**: Consider time-based partitioning for large datasets
- **Connection Pooling**: Handle concurrent requests efficiently

#### Performance:
- **High Throughput**: Handle 1000+ log entries per second
- **Low Latency**: Sub-100ms response times for search queries
- **Caching**: Redis caching for frequently accessed data
- **Async Processing**: Background tasks for data archival and cleanup

#### Security:
- **Data Encryption**: Encrypt sensitive log data at rest
- **Access Control**: Fine-grained permissions for log access
- **Audit Trail**: Log access to the audit logs themselves
- **Data Masking**: Mask sensitive information in logs (PII, passwords)

### Implementation Details:

#### Technology Stack:
- **Framework**: Django or FastAPI
- **Database**: PostgreSQL with TimescaleDB extension (for time-series data) or MongoDB
- **Cache**: Redis
- **Message Queue**: Celery for background tasks
- **Search**: Elasticsearch (optional, for advanced search capabilities)

#### API Endpoints:
```
POST   /api/v1/logs                    # Create log entry
GET    /api/v1/logs                    # Search/filter logs
GET    /api/v1/logs/{id}              # Get specific log entry
GET    /api/v1/logs/export            # Export logs
GET    /api/v1/logs/stats             # Get log statistics
POST   /api/v1/logs/bulk              # Bulk log creation
DELETE /api/v1/logs/cleanup           # Cleanup old logs
WS     /api/v1/logs/stream            # Real-time log streaming
```

#### System Architecture:
```mermaid
graph TB
    subgraph "Client Applications"
        App1[Application 1]
        App2[Application 2]
        App3[Application 3]
    end
    
    subgraph "Audit Log API"
        API[API Gateway]
        Auth[Authentication]
        RateLimit[Rate Limiting]
    end
    
    subgraph "Core Services"
        LogService[Log Service]
        SearchService[Search Service]
        ExportService[Export Service]
        StreamService[Stream Service]
    end
    
    subgraph "Data Layer"
        PostgreSQL[(PostgreSQL)]
        Redis[(Redis Cache)]
        Elasticsearch[(Elasticsearch)]
    end
    
    subgraph "Background Services"
        Celery[Celery Workers]
        Cleanup[Data Cleanup]
        Archive[Data Archival]
    end
    
    App1 --> API
    App2 --> API
    App3 --> API
    
    API --> Auth
    Auth --> RateLimit
    RateLimit --> LogService
    RateLimit --> SearchService
    RateLimit --> ExportService
    RateLimit --> StreamService
    
    LogService --> PostgreSQL
    SearchService --> Elasticsearch
    SearchService --> Redis
    ExportService --> PostgreSQL
    
    Celery --> PostgreSQL
    Cleanup --> PostgreSQL
    Archive --> PostgreSQL
```

#### Audit Log Flow:
```mermaid
sequenceDiagram
    participant Client as Client App
    participant API as API Gateway
    participant Auth as Auth Service
    participant Log as Log Service
    participant DB as PostgreSQL
    participant Cache as Redis
    participant Search as Elasticsearch
    
    Client->>API: POST /api/v1/logs
    API->>Auth: Validate JWT Token
    Auth-->>API: Token Valid
    API->>Log: Create Log Entry
    Log->>DB: Store Log Entry
    Log->>Cache: Cache Recent Logs
    Log->>Search: Index for Search
    Log-->>API: Log Created
    API-->>Client: 201 Created
    
    Note over Client,Search: Real-time Streaming
    Client->>API: WS /api/v1/logs/stream
    API->>Log: Subscribe to Stream
    Log->>Client: Real-time Log Updates
```

### Testing:

- **Unit Tests**: >85% code coverage
- **Integration Tests**: API endpoint testing
- **Performance Tests**: Load testing with realistic data volumes
- **Security Tests**: Authentication and authorization testing

### Documentation:

- **API Documentation**: OpenAPI/Swagger documentation
- **Setup Instructions**: Clear deployment and configuration guide
- **Architecture Diagram**: System design and data flow
- **Code Documentation**: Inline comments and docstrings

### Bonus Features (Optional):

- **Alert System**: Configure alerts for specific log patterns
- **Dashboard**: Simple web interface for log visualization
- **Log Analytics**: Basic analytics and reporting
- **Multi-tenancy**: Support for multiple applications/tenants
- **Log Correlation**: Group related log entries by request ID

### Submission:

- **Git Repository**: Clean, well-structured code
- **README**: Comprehensive setup and usage instructions
- **API Documentation**: Complete endpoint documentation
- **Postman Collection**: Test the API endpoints
- **Architecture Diagram**: System design overview
- **Live Demo**: Deployed application (optional)

### Evaluation Criteria:

#### Code Quality & Architecture (30%):
- **Code Structure**: Clean, maintainable, and well-structured code
- **API Design**: RESTful principles, proper error handling, validation
- **Database Design**: Efficient schema design and query optimization
- **Technical Decisions**: Justification of technology choices and architecture

#### Performance & Scalability (25%):
- **High Throughput**: Ability to handle 1000+ log entries per second
- **Low Latency**: Sub-100ms response times for search queries
- **Caching Strategy**: Effective use of Redis and other caching mechanisms
- **Database Optimization**: Efficient indexes and query performance

#### Security & Compliance (20%):
- **Authentication**: Proper JWT-based authentication implementation
- **Authorization**: Role-based access control and fine-grained permissions
- **Data Protection**: Encryption at rest and in transit
- **Input Validation**: Proper sanitization and validation of all inputs

#### Testing & Documentation (15%):
- **Test Coverage**: >85% code coverage with comprehensive testing
- **API Documentation**: Complete OpenAPI/Swagger documentation
- **Setup Instructions**: Clear deployment and configuration guide
- **Code Documentation**: Inline comments and comprehensive docstrings

#### Problem Solving & Innovation (10%):
- **Complex Requirements**: Ability to handle complex requirements efficiently
- **Creative Solutions**: Innovative approaches to technical challenges
- **Bonus Features**: Implementation of optional advanced features
- **Performance Optimization**: Creative solutions for performance challenges

## Timeline:

This challenge is designed to be completed in **3-5 business days**:

#### **Days 1-2: Core API Development**
- Set up project structure and technology stack
- Implement core audit log management features
- Design and implement database schema
- Create basic API endpoints

#### **Days 3-4: Advanced Features & Testing**
- Implement search, filtering, and export functionality
- Add real-time streaming capabilities
- Implement security and authentication
- Create comprehensive test suite

#### **Day 5: Documentation & Final Review**
- Complete API documentation
- Create architecture diagrams
- Final testing and optimization
- Prepare submission materials

Focus on delivering a working MVP with core features rather than implementing all bonus features.

## Priority Focus Areas:

#### **Must Complete (High Priority):**
- Core audit log creation and retrieval API endpoints
- Basic search and filtering functionality
- Database schema design and implementation
- Authentication and authorization system
- Basic security controls and data validation

#### **Should Complete (Medium Priority):**
- Advanced search with full-text capabilities
- Real-time log streaming via WebSocket
- Data retention and archival policies
- Performance optimization and caching
- Comprehensive test coverage

#### **Nice to Have (Low Priority):**
- Export functionality (JSON, CSV)
- Dashboard and visualization interface
- Advanced analytics and reporting
- Multi-tenancy support
- Alert system for log patterns

## Questions?

Any questions you may have, please contact us by e-mail.

Good luck! 🚀
