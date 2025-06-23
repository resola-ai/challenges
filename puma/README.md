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

- Coverage: The extent to which the API endpoints are tested.
- Correctness: The accuracy of the tests in validating API responses and handling edge cases.
- Performance: The response times of the API endpoints are within acceptable limits.
- Security: The API should not be vulnerable to basic security threats.
- Automation: The degree to which the tests can be automated and integrated into a CI/CD pipeline.
- Documentation: Clarity and completeness of the documentation.

Good luck, and happy testing!
