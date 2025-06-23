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

- **Code Quality**: Clean, maintainable, and well-structured code
- **API Design**: RESTful principles, proper error handling, validation
- **Performance**: Efficient database queries, caching, and optimization
- **Security**: Proper authentication, authorization, and data protection
- **Testing**: Comprehensive test coverage and quality
- **Documentation**: Clear and complete documentation
- **Problem Solving**: Ability to handle complex requirements efficiently
- **Technical Decisions**: Justification of technology choices

## Timeline:

This challenge is designed to be completed in **24 hours to 1 week** depending on the candidate's experience level. Focus on delivering a working MVP with core features rather than implementing all bonus features.

## Questions?

Any questions you may have, please contact us by e-mail.

Good luck! 🚀
