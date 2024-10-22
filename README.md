
# Rule Engine Application

## Overview

This application is a rule engine that determines user eligibility based on attributes such as age, department, salary, and experience. It uses an Abstract Syntax Tree (AST) to represent and manage conditional rules, allowing for dynamic rule creation, combination, and evaluation.

The application features a simple UI, API, and backend to manage rule logic and data.

## Features

- **Create Rules:** Define rules using string-based conditions that are parsed and converted into AST structures.
- **Example:** "((age > 30 AND department = 'Sales') OR (age < 25 AND department = 'Marketing')) AND (salary > 50000 OR experience > 5)"
- **Combine Rules:** Merge multiple rules into a single AST for complex evaluations, minimizing redundant checks.
- **Evaluate Rules:** Evaluate the AST rules against JSON input data to determine eligibility based on the defined conditions.
- **Dynamic Tree Representation:** Visualize the AST structure of the rules for easy understanding and debugging.

## Tech Stack

- **Backend:** Node.js, Express.js
- **Database:** MongoDB
- **Frontend:** Basic HTML/CSS for UI

## Getting Started

### Prerequisites

- Node.js and npm installed
- MongoDB installed and running

### Installation Guide

1. **Clone the Repository**
   ```bash
   git clone "https://github.com/Siddhu2668/Rule-Engine-with-AST.git"
   cd Rule-Engine-with-AST
   ```

2. **Install Backend Dependencies**
   ```bash
   npm install
   ```

3. **Start MongoDB**
   Ensure that MongoDB is running on your local machine:
   ```bash
   mongod
   ```

4. **Start the Backend Server**
   ```bash
   nodemon server.js
   or
   npm start
   ```

## API Endpoints

1. **Create a Rule**
   - **Endpoint:** `/api/create_rule`
   - **Method:** POST
   - **Description:** Takes a string representation of a rule and converts it into an AST.
   - **Request Body:**
     ```json
     {
       "ruleString": "((age > 30 AND department = 'Sales') OR (age < 25 AND department = 'Marketing')) AND (salary > 50000 OR experience > 5)",
       "ruleName": "Rule 1"
     }
     ```
     Use appropriate spaces in rules for correct results.
   - **Response:**
     ```json
     {
       "_id": "605c72ef1f4e3a001f4d2e9a",
       "rule_name": "Rule1",
       "rule_ast": { /* AST structure */ }
     }
     ```

2. **Combine Rules**
   - **Endpoint:** `/api/rules/combine_rules`
   - **Method:** POST
   - **Description:** Combines multiple rules into a single AST.
   - **Request Body:**
     ```json
     {
       "ruleIds": ["605c72ef1f4e3a001f4d2e9a", "605c730f1f4e3a001f4d2e9b"],
       "operators": "AND"
     }
     ```
   - **Response:**
     ```json
     {
       "type": "operator",
       "value": "AND",
       "left": { /* Rule 1 AST */ },
       "right": { /* Rule 2 AST */ }
     }
     ```

3. **Evaluate a Rule**
   - **Endpoint:** `/api/rules/evaluate_rule`
   - **Method:** POST
   - **Description:** Evaluates a rule's AST against user data.
   - **Request Body:**
     ```json
     {
       "rule": { /* AST structure */ },
       "data": {
         "age": 35,
         "department": "Sales",
         "salary": 60000,
         "experience": 3
       }
     }
     ```
   - **Response:**
     ```json
     {
       "result": true
     }
     ```

## Data Structure

The rule engine uses an Abstract Syntax Tree (AST) to represent rules. Each node in the tree can either be:

- **Operator:** Represents logical operations (AND, OR).
- **Operand:** Represents conditions (age > 30, salary > 50000).

- **Node Structure:**
  ```json
  {
    "type": "operator",
    "value": "AND", // OR for operators, comparison for operands
    "left": { /* Left child node */ },
    "right": { /* Right child node */ }
  }
  ```

## Example of Rule1 AST

For the rule `((age > 30 AND department = 'Sales') OR (age < 25 AND department = 'Marketing')) AND (salary > 50000 OR experience > 5)`, the corresponding AST would look something like this:

```json
{
  "type": "operator",
  "value": "AND",
  "left": {
    "type": "operator",
    "value": "OR",
    "left": {
      "type": "operator",
      "value": "AND",
      "left": { "type": "operand", "value": "age > 30" },
      "right": { "type": "operand", "value": "department = 'Sales'" }
    },
    "right": {
      "type": "operator",
      "value": "AND",
      "left": { "type": "operand", "value": "age < 25" },
      "right": { "type": "operand", "value": "department = 'Marketing'" }
    }
  },
  "right": {
    "type": "operator",
    "value": "OR",
    "left": { "type": "operand", "value": "salary > 50000" },
    "right": { "type": "operand", "value": "experience > 5" }
  }
}
```

## Database Schema

### Rule Schema

- **rule_name:** String, name of the rule
- **rule_ast:** Object, the AST representation of the rule

- **MongoDB Sample Entry:**
  ```json
  {
    "_id": "605c72ef1f4e3a001f4d2e9a",
    "rule_name": "Rule 1",
    "rule_ast": { /* AST representation */ }
  }
  ```

## Testing

To ensure the correctness of the implementation, the following test cases should be executed:

- **Create Individual Rules:** Use the `create_rule` API to generate an AST for a single rule and verify its correctness.
- **Combine Rules:** Test the combination of multiple rules using the `combine_rules` API and check that the resulting AST is accurate.
- **Evaluate Rules:** Run the `evaluate_rule` API against various user data scenarios and verify that the correct boolean result is returned.
- **Error Handling:** Ensure the system gracefully handles invalid rule strings, data format issues, or missing attributes.

## Bonus Features

- **Error Handling:** Graceful management of invalid rule strings, operators, and data.
- **Dynamic Modifications:** The ability to update existing rules, change operators, add/remove conditions dynamically.
- **User-defined Functions:** Future extension to support custom user-defined functions for more complex logic.

## Conclusion

This rule engine with AST provides a flexible, dynamic, and scalable approach to manage eligibility criteria. The modular structure allows for future enhancements and more complex rules, making it ideal for various decision-making systems.

## Running Tests

You can add and run tests to ensure everything is working correctly.
