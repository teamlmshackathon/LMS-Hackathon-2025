# LMS Hackathon 2025 API Testing

This repository contains the Postman collection, environment configuration, and data-driven tests for the **LMS Hackathon 2025** API. The tests are designed to be executed using **Newman** (Postman's CLI runner) in a specific sequence to handle authentication and dependency chaining.

## 📂 Repository Structure

Ensure your files are organized in the following directory structure to match the execution scripts:

```text
.
├── Postman Collections/
│   └── LMS-Hackathon-2025.json               # Main Postman Collection
├── Test Data/
│   ├── Test_Data_User_Login_Controller.csv
│   ├── Test_Data_Program_Controller.csv
│   ├── Test_Data_Program_Batch_controller.csv
│   ├── Test_Data_User_Controller.csv
│   └── Test_Data_Skill_Master_Controller.csv
└── QA_Environment.json                       # Base Environment (contains baseUrl)
```

## 🛠️ Prerequisites

*   **Node.js** (v16+)
*   **npm** (Node Package Manager)
*   **Jenkins** (for CI/CD execution)

## 🚀 Local Installation

1.  **Clone the repository:**
    ```bash
    git clone <your-repo-url>
    cd <your-repo-folder>
    ```

2.  **Install dependencies:**
    ```bash
    npm install newman newman-reporter-htmlextra
    ```

## ⚙️ Execution Strategy

This collection requires a specific execution order because the **UserLogin** folder generates a Bearer Token that is required by all subsequent folders.

1.  **UserLogin**: Authenticates and saves the `token` to a temporary environment file.
2.  **ProgramController**: Consumes the token.
3.  **Program Batch Controller**: Consumes the token.
4.  **UserController**: Consumes the token.
5.  **SkillMaster**: Consumes the token.

---

## 🤖 Jenkins Integration

To run this suite in Jenkins, use a **Freestyle Project** with the following configuration.

### 1. Build Environment
*   Check **"Provide Node & npm bin/ folder to PATH"**.
*   Select your NodeJS installation (e.g., NodeJS 16/20).

### 2. Execute Shell Script
Paste the following script into the **Build > Execute shell** window. This script handles dependency installation and the chained execution flow.

```bash
#!/bin/bash
set -e

# --- 1. Install Dependencies ---
echo "--- Installing Newman and Reporter ---"
npm install newman newman-reporter-htmlextra

# --- 2. Define Paths ---
COL="Postman Collections/LMS-Hackathon-2025.json"
BASE_ENV="QA_Environment.json"
TEMP_ENV="newman/env.json"

# --- 3. Clean Previous Reports ---
rm -rf newman
mkdir -p newman

# --- 4. Run Test Suite ---

# Step 1: Login & Token Capture
echo "--- STARTING: UserLogin ---"
./node_modules/.bin/newman run "$COL" \
    -e "$BASE_ENV" \
    --folder "UserLogin" \
    -d "Test Data/Test_Data_User_Login_Controller.csv" \
    --reporters cli,htmlextra \
    --reporter-htmlextra-export "newman/report-1-login.html" \
    --export-environment "$TEMP_ENV"

# Step 2: Program Controller
echo "--- STARTING: ProgramController ---"
./node_modules/.bin/newman run "$COL" \
    --folder "ProgramController" \
    -d "Test Data/Test_Data_Program_Controller.csv" \
    --reporters cli,htmlextra \
    --reporter-htmlextra-export "newman/report-2-program.html" \
    --environment "$TEMP_ENV"

# Step 3: Program Batch Controller
echo "--- STARTING: Program Batch Controller ---"
./node_modules/.bin/newman run "$COL" \
    --folder "Program Batch Controller" \
    -d "Test Data/Test_Data_Program_Batch_controller.csv" \
    --reporters cli,htmlextra \
    --reporter-htmlextra-export "newman/report-3-batch.html" \
    --environment "$TEMP_ENV"

# Step 4: User Controller
echo "--- STARTING: UserController ---"
./node_modules/.bin/newman run "$COL" \
    --folder "UserController" \
    -d "Test Data/Test_Data_User_Controller.csv" \
    --reporters cli,htmlextra \
    --reporter-htmlextra-export "newman/report-4-user.html" \
    --environment "$TEMP_ENV"

# Step 5: Skill Master
echo "--- STARTING: SkillMaster ---"
./node_modules/.bin/newman run "$COL" \
    --folder "SkillMaster" \
    -d "Test Data/Test_Data_Skill_Master_Controller.csv" \
    --reporters cli,htmlextra \
    --reporter-htmlextra-export "newman/report-5-skill.html" \
    --environment "$TEMP_ENV"
```

### 3. Post-Build Actions
*   Add **"Publish HTML reports"**.
*   **Directory**: `newman`
*   **Index Pages**: `report-1-login.html, report-2-program.html, report-3-batch.html, report-4-user.html, report-5-skill.html`

## 📄 License
This project is part of the LMS Hackathon 2025.
