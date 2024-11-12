## Testing Guidelines
This document outlines the best practices for **Developers** before it reaches the **QA team** or production within our organization.

## Overview
Writing test cases during development is essential for maintaining code quality and ensuring that features and functionalities work as expected.

## Here’s how developers generally approach testing:

#### 1. Unit Testing
- **Purpose:** To test individual functions, methods, or components in isolation.
- **Tools:** Popular frameworks include **Jest (JavaScript/TypeScript), JUnit (Java), PHPUnit (PHP), and PyTest (Python)**.
- **Example:** A developer might write unit tests to validate that a function for calculating a hotel room price returns the correct amount given various inputs.

  ```
  // Testing a function that calculates the sum of two numbers.
  
  function sum(a, b) {
    return a + b;
  }
  
  // Jest Unit Test
  test('sum function adds two numbers correctly', () => {
    expect(sum(2, 3)).toBe(5);
    expect(sum(-1, 1)).toBe(0);
    expect(sum(0, 0)).toBe(0);
  });
  
  ```

#### 2. Component Testing (Frontend Development)
- **Purpose:** To test individual UI components in isolation to ensure they render and behave correctly.
- **Tools:** **React Testing Library**, **Vue Test Utils**, and **Cypress** (for end-to-end but can be used for components).
- **Example:** Ensuring a React modal dialog opens and closes as expected when buttons are clicked.

  ```
  // Button.js
  import React from 'react';
  
  const Button = ({ onClick, label }) => (
    <button onClick={onClick}>{label}</button>
  );
  
  export default Button;
  
  // Button.test.js
  import { render, screen, fireEvent } from '@testing-library/react';
  import Button from './Button';
  
  test('renders the button with the given label', () => {
    render(<Button label="Click me" onClick={() => {}} />);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });
  
  test('calls the onClick handler when clicked', () => {
    const handleClick = jest.fn();
    render(<Button label="Click me" onClick={handleClick} />);
    const button = screen.getByText('Click me');
  
    fireEvent.click(button);
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
  
  ```

#### 3. Integration Testing
- **Purpose:** To test how different modules or services work together.
- **Tools:** **Mocha with Chai** (JavaScript/TypeScript), **Postman** or **REST-assured** for API testing, and **TestNG** (Java).
- **Example:** Checking if a booking component communicates correctly with the payment processing service.

  ```
  // Testing an API endpoint that fetches user data and saves it to a database.
  
  const request = require('supertest');
  const app = require('../app'); // your Express app
  
  describe('GET /user', () => {
    it('should fetch user data and save it to the database', async () => {
      const response = await request(app).get('/user/123');
      expect(response.status).toBe(200);
      expect(response.body).toHaveProperty('name');
      // Check that the data is correctly saved in the database (mocked)
    });
  });
  
  ```

#### 4. End-to-End (E2E) Testing
- **Purpose:** To simulate real user scenarios from start to finish and verify that the entire application works as expected.
- **Tools:** **Cypress, Selenium, Playwright**.
- **Example:** Automating a test where a user logs in, makes a reservation, and receives confirmation.

  ```
  // Simulating user behavior like logging in and creating a post.
  
  // Cypress E2E test
  describe('User login and create post', () => {
    it('should log in and create a new post successfully', () => {
      cy.visit('/login');
      cy.get('input[name="username"]').type('testuser');
      cy.get('input[name="password"]').type('password');
      cy.get('button[type="submit"]').click();
      
      cy.url().should('include', '/dashboard');
      cy.get('button.new-post').click();
      cy.get('textarea.post-content').type('This is a new post');
      cy.get('button.submit-post').click();
      
      cy.contains('Post created successfully');
    });
  });
  
  ```

#### 5. Manual Testing
- **Purpose:** Developers often manually run through key parts of the application to catch issues not covered by automated tests.
- **Example:** Common for smaller features or quick checks. Developers may run the app, perform actions like clicking buttons, submitting forms, and verifying that UI elements behave as expected.

#### 6. Debugging and Ad-hoc Testing
- **Purpose:** While coding or after a bug is reported, developers will frequently perform impromptu testing by running specific parts of the application and using debugging tools.
- **Tools:** **Built-in IDE debuggers**, **browser developer tools**, and **console logging**.
- **Example:** Set breakpoints, inspect variable states, and follow the code flow to understand and fix bugs.

#### 7. Static Code Analysis
- **Purpose:** To find potential bugs or performance issues without executing the code.
- **Tools:** **ESLint** (JavaScript), SonarQube, **Prettier**, and other **linters** or **formatters**.
- **Example:** Detecting unused variables, syntax errors, potential security vulnerabilities, or style inconsistencies.

  ```
  // Running ESLint in a React project.
  
  eslint src/
  ```

#### 8. Mocking and Stubbing
- **Purpose:** To isolate the part of the code being tested by replacing dependencies with mock versions.
- **Tools:** Sinon.js, Jest mocks, Mock Service Worker (for API responses).
- **Example:** Mocking an API response to test how the UI behaves when the server returns an error.

#### 9. Performance Testing (Basic)
- **Purpose:** Developers might do simple performance checks, such as how long a function takes to execute or if a page renders efficiently.
- **Tools:** **Lighthouse** (for frontend), custom logging.
- **Example:** Checking if an API call completes within acceptable time limits.

  ```
  lighthouse https://example.com --view
  ```

## Test-Driven Development: 
A software development methodology where tests are written before the code itself. The code is developed incrementally by first writing a failing test, then implementing just enough code to make the test pass, and finally refactoring while keeping the tests green.

- **TDD:** Development follows a strict cycle known as Red-Green-Refactor:
  - **Red:** Write a test that fails (because the feature isn't implemented yet).
  - **Green:** Write the minimum code necessary to pass the test.
  - **Refactor:** Improve the code while keeping the test passing.

- **Workflow**
  - **Unit Testing:**
    - Write code.
    - Write unit tests for that code.
    - Run the tests and fix any issues.
  - **TDD:**
    - Write a test that defines a desired function or improvement (fails initially).
    - Implement the minimum code to make the test pass.
    - Run the test to confirm it passes.
    - Refactor the code to optimize it while ensuring the test still passes.
    - Repeat for the next feature or function.
   
  #### TDD Example:
  - **Red:** Write a failing test.
      ```
      test('adds 2 and 3 to equal 5', () => {
        expect(add(2, 3)).toBe(5);
      });
      ```
  - **Green:** Write just enough code to pass the test.
      ```
      function add(a, b) {
        return a + b;
      }
      ```
  - **Refactor:** Improve the code while keeping the test passing. In this case, if there’s no optimization needed, this step may be skipped.
    


## Things needs to consider while writing the test cases
### 1. Understand the Requirements:
  - Clearly understand what the feature or function is supposed to do. 
  - Identify edge cases and expected outcomes for different input scenarios. 

### 2. Choose the Right Testing Framework
 #### Select a suitable testing framework based on your project’s technology stack: 
- **JavaScript/TypeScript (React, Node.js):** Jest, Mocha, Cypress 
- **Java (Spring Boot):** JUnit, Mockito 
- **PHP (Laravel):** PHPUnit

 ### 3. Follow a Consistent Structure
#### Test cases should have a clear format:
- **Test Name:** Describes what the test is checking (e.g., should return 200 OK when the user is authenticated).
- **Setup:** Prepares the environment or input for the test.
- **Execution:** Calls the function or triggers the feature.
- **Assertion:** Verifies the result matches the expected outcome.
- **Teardown (optional):** Cleans up after the test if needed.

### Test case structure
**Here’s a comprehensive structure for documenting test cases:**

| Field  | Description |
| ------------- | ------------- |
| Test Case ID  | Unique identifier (e.g., TC001, TC002).  |
| Title  | Brief description of the test (e.g., Verify login).  |
| Description  | Detailed explanation of the test's purpose. |
| Preconditions  | Any setup needed before running the test.  |
| Test Steps  | Step-by-step instructions for executing the test.  |
| Test Data  | Specific data required (e.g., user credentials).  |
| Expected Result  | Expected outcome if the test passes.  |
| Actual Result  | Record of what happens when the test is run.  |
| Status  | Pass/Fail status.  |
| Comments  | Additional observations or notes.  |
| Tested By  | Name of the tester.  |
| Date Executed  | The date the test was executed.  |

### 4. Document Test Cases
- Keep a log of test cases and document them for easy reference.
- Maintain descriptions, expected inputs/outputs, and scenarios in a shared space.


## Video Referances:
- **Levels of Testing:** - https://youtu.be/YaXJeUkBe4Y?si=efn4zicg0w4ej6FC



