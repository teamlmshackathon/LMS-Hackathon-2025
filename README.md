# LMS-Hackathon-2025

# LMS API Automation Testing Project (Hackathon 2025)

## 📌 Project Overview
This repository contains the automated API testing suite for the **Skill Master Controller** module of the Learning Management System (LMS). The project utilizes **Postman** for request creation and **Newman** for command-line execution and reporting.

The test suite is designed to validate:
- **CRUD Operations:** Verify Create, Read, Update, and Delete functionalities for Skills.
- **Data Validation:** Ensure strict validation for invalid inputs (e.g., NULL values, empty strings, SQL injection attempts).
- **Security Testing:** Validate proper handling of `401 Unauthorized` (Invalid Token) and `405 Method Not Allowed` scenarios.
- **Schema Validation:** Ensure API responses strictly adhere to the defined JSON contracts.

---

## 📂 Repository Structure
- **`LMS-Hackathon-2025.postman_collection.json`**: The Postman Collection containing all API requests and test scripts (Pre-request & Tests).
- **`Test_Data_Skill_Master_Controller.csv`**: The Data-Driven testing file containing positive and negative test scenarios.
- **`Newman-Report-Skill-Master-Controller.html`**: A comprehensive HTML execution report showing the results of the test run.

---

## 🚀 Technologies Used
- **Postman**: For API development and initial testing.
- **Newman**: For running collections via CLI and CI/CD pipelines.
- **JavaScript (Chai Assertion Library)**: For writing validation scripts.
- **CSV Data Driving**: For testing multiple scenarios dynamically.

---

## ⚙️ Setup & Installation

### Prerequisites
1. **Node.js** installed on your machine.
2. **Newman** installed globally:  npm install -g newman
3. **Newman HTML Reporter** (for generating the report):  npm install -g newman-reporter-htmlextra


### Running the Tests
To execute the test suite locally, navigate to the project folder and run the following command:  newman run LMS-Hackathon-2025.postman_collection.json
-d Test_Data_Skill_Master_Controller.csv
-r htmlextra
--reporter-htmlextra-export "Newman-Report-Skill-Master-Controller.html"


*Note: You may need to update the `token` and `baseURL` variables in the Collection JSON file or pass them via an Environment file.*

---

## 📊 Test Scenarios Covered
The `Test_Data_Skill_Master_Controller.csv` file drives the following test cases:

### ✅ Positive Scenarios (Happy Path)
- **Create Skill:** Successfully create a new skill with valid data.
- **Get All Skills:** Retrieve the full list of skills.
- **Get Skill by ID/Name:** Validate data retrieval accuracy.
- **Update Skill:** Modify existing skill details.
- **Delete Skill:** Remove a skill and verify it no longer exists.

### ❌ Negative Scenarios (Edge Cases)
- **Duplicate Data:** Attempting to create a skill that already exists (Expect `400`).
- **Invalid Inputs:** Sending `NULL`, empty strings, or whitespace-only names.
- **Injection Attacks:** Testing for SQL and HTML injection vulnerabilities (Expect `400`).
- **Invalid IDs:** Updating/Deleting with non-existent, negative, or zero IDs (Expect `404` or `400`).

### 🔒 Security Scenarios
- **Unauthorized Access:** Accessing endpoints with an invalid Bearer Token (Expect `401`).
- **Method Not Allowed:** Sending `POST` requests to `GET` endpoints, etc. (Expect `405`).

---

## 📈 Reporting
The execution generates a detailed **HTML Report** (`Newman-Report-Skill-Master-Controller.html`) which includes:
- **Summary:** Total assertions, failed tests, and skipped tests.
- **Request Details:** Request Body, Response Body, and Headers for debugging.
- **Pass/Fail Status:** Clear visual indicators for every assertion.

---

## 📝 Notes for Reviewers
- **Token Security:** The Bearer Token in the HTML report has been redacted/sanitized for security purposes.
- **Dynamic Logic:** The collection uses advanced Pre-request scripts to handle "Method Overrides" (e.g., forcing a `GET` request to become `POST` to test 405 errors) based on the CSV input.

---

**Author:** Tanuja Narravula
**Hackathon:** LMS Hackathon 2025





