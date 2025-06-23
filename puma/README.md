# Code Challenge: API Testing with Postman

### Objective:

Your task is to develop a comprehensive set of Postman tests to validate the functionality, reliability, performance, and security of the 'Fake Store API' as documented here: https://documenter.getpostman.com/view/10186709/2s9YXfd4QF.

## Requirements:

### Understanding the API:

- Review the API documentation thoroughly to understand the available endpoints, request methods, and expected responses.

### Functional Testing:

- Create a Postman Collection for the API.
- Write tests for each endpoint to verify that it responds with the correct HTTP status codes and response bodies. For example:
  - GET requests should return a 200 OK status with the correct data.
  - POST requests should successfully create resources and return a 201 Created status.
  - PUT and DELETE requests should modify and remove resources, respectively, and return appropriate status codes.
- Validate the response structure using JSON schema validation where applicable.
- Ensure that required fields in the request payload are validated.
- Test for successful handling of edge cases and input validations (e.g., negative tests for invalid data).

### Performance Testing:

- Create tests to benchmark the response time of the API endpoints. Establish a baseline for acceptable performance and ensure that the API meets these benchmarks.

### Security Testing:

- Test for common security vulnerabilities such as SQL injection, Cross-Site Scripting (XSS), and ensure that sensitive information is not exposed in responses.
- Verify that authentication is required and enforced for protected endpoints.

### Automation:

- Utilize Postman's Collection Runner to automate your test suite.
- Include pre-request scripts if needed to set up any prerequisites for the tests.
- Use Postman's environment variables to handle different testing environments (e.g., staging vs production).

### Reporting:

- Generate and provide test reports that summarize the test results, including the number of tests passed/failed and any identified issues.
- Utilize Newman, Postman's command-line Collection Runner, to integrate your tests with a CI/CD pipeline if applicable.

### Documentation:

- Document your testing strategy, including the types of tests conducted, any assumptions made, and the rationale behind your test cases.
- Provide clear instructions on how to execute the test collection and any environment setup required.

### Deliverables:

- A Postman Collection file (.json) containing all the tests.
- A test report generated from your test execution.
- Documentation detailing your testing approach and instructions for running the tests.

### Evaluation Criteria:

#### Test Coverage (30%):
- **API Endpoint Coverage**: Complete testing of all available endpoints
- **Edge Case Testing**: Comprehensive testing of error conditions and invalid inputs
- **Data Validation**: Proper validation of request/response data structures
- **Negative Testing**: Testing for failure scenarios and error handling

#### Test Quality (25%):
- **Test Accuracy**: Correctness of test assertions and expected outcomes
- **Test Reliability**: Consistent and repeatable test execution
- **Test Maintainability**: Well-organized and documented test collection
- **Automation**: Effective use of Postman automation features

#### Performance Testing (20%):
- **Response Time Validation**: Proper benchmarking and performance criteria
- **Load Testing**: Testing under realistic load conditions
- **Performance Metrics**: Meaningful performance measurements and reporting
- **Baseline Establishment**: Clear performance benchmarks and thresholds

#### Security Testing (15%):
- **Vulnerability Testing**: Testing for common security vulnerabilities
- **Authentication Testing**: Proper testing of authentication mechanisms
- **Data Exposure Testing**: Verification of sensitive data protection
- **Security Best Practices**: Implementation of security testing methodologies

#### Documentation & Reporting (10%):
- **Test Documentation**: Clear documentation of testing approach and strategy
- **Test Reports**: Comprehensive and actionable test reports
- **Setup Instructions**: Clear instructions for test execution
- **Code Quality**: Clean, well-structured test collection

## Timeline:

This challenge is designed to be completed in **2-3 business days**:

#### **Day 1: API Analysis & Test Planning**
- Review API documentation thoroughly
- Create test strategy and plan
- Set up Postman environment and variables

#### **Day 2: Test Implementation**
- Create Postman collection
- Implement functional tests for all endpoints
- Add performance and security tests

#### **Day 3: Automation & Documentation**
- Set up automation with Collection Runner
- Generate test reports
- Complete documentation and final review

## Priority Focus Areas:

#### **Must Complete (High Priority):**
- Functional testing of all API endpoints
- Basic performance testing with response time validation
- Security testing for common vulnerabilities
- Test automation with Postman Collection Runner
- Basic test documentation and reporting

#### **Should Complete (Medium Priority):**
- Comprehensive edge case testing
- Advanced performance testing with load simulation
- Detailed security vulnerability assessment
- Test environment configuration and variables
- Integration with CI/CD pipeline

#### **Nice to Have (Low Priority):**
- Advanced test reporting with custom dashboards
- Performance benchmarking with detailed metrics
- Comprehensive security testing suite
- Test data management and cleanup
- Advanced Postman scripting and automation

## Questions?

Any questions you may have, please contact us by e-mail.

Good luck! 🚀
