# GPT-4o Auto-Issue Generator

An AI-driven GitHub Action for automated code review, security auditing, and intelligent issue tracking utilizing the OpenAI GPT-4o API.

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen?style=for-the-badge)](https://github.com/OstinUA)
[![Version](https://img.shields.io/badge/version-1.0.0-blue?style=for-the-badge)](https://github.com/OstinUA)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)
[![Python 3.11](https://img.shields.io/badge/python-3.11-blue.svg?style=for-the-badge)](https://www.python.org/downloads/release/python-3110/)

## Table of Contents
1. [Features](#features)
2. [Tech Stack & Architecture](#tech-stack--architecture)
3. [Getting Started](#getting-started)
4. [Testing](#testing)
5. [Deployment](#deployment)
6. [Usage](#usage)
7. [Configuration](#configuration)
8. [License](#license)
9. [Support the Project](#support-the-project)

## Features
* **Automated Diff Analysis:** Extracts and parses code patches from Git pushes and Pull Requests.
* **Intelligent Labeling:** Automatically assigns relevant tags (e.g., `bug`, `enhancement`, `security`) based on context.
* **Security Auditing:** Scans patches for hardcoded secrets, injection flaws, and XSS vulnerabilities, appending a `Security Warning` to the issue body.
* **Dual Trigger Support:** Seamlessly handles both standard branch pushes and PR synchronization events without duplicating issues.
* **Identity Verification:** Executes workflows exclusively for the designated repository owner to prevent unauthorized API usage.

## Tech Stack & Architecture
* **Language:** Python 3.11
* **Orchestration:** GitHub Actions (IssueOps pattern)
* **Libraries:** `PyGithub` (for GitHub REST API interactions), `openai` (for OpenAI API communication)
* **LLM Provider:** OpenAI API (`gpt-4o` model)

### Project Structure & Key Design Decisions
The architecture follows a decoupled IssueOps model. The `.yml` file handles environment provisioning and CI/CD triggers, while the state-agnostic Python script manages logic.
* `.github/workflows/ai-issue.yml`: CI/CD pipeline definition. Injects secrets as environment variables.
* `process_event.py`: Core logic runner. Uses the official OpenAI Python SDK to communicate with the GPT-4o model.

## Getting Started

### Prerequisites
* A GitHub repository with GitHub Actions enabled.
* An OpenAI API key.

### Installation
1. Clone your repository:
   ```bash
   git clone https://github.com/OstinUA/your-repo.git
   cd your-repo
   ```

2. Create the GitHub Action workflow file `.github/workflows/ai-issue.yml` and paste the provided YAML configuration.
3. Create `process_event.py` in the root directory and paste the provided Python script.
4. Commit and push the files to your `main` branch:
   ```bash
   git add .
   git commit -m "chore: setup gpt-4o auto-issue generator"
   git push origin main
   ```

## Testing

To test the integration locally or trigger a dry-run:

1. Create a dummy branch: `git checkout -b test-ai-action`
2. Introduce a deliberate flaw (e.g., `const API_KEY = "12345";`).
3. Push the branch and open a Pull Request to `main`.
4. Monitor the Actions tab to verify successful execution and issue generation.

## Deployment

Deployment is handled automatically via GitHub Actions upon pushing the `.yml` configuration to the default branch. Ensure that repository workflow permissions are set to `Read and write permissions`.

## Usage

The system operates autonomously in the background.

* **For Pushes:** Push commits directly to `main`. An issue will be generated documenting the changes.
* **For PRs:** Open or synchronize a Pull Request. The action will analyze the entire PR diff and generate a summary issue.

## Configuration

Configure the following Repository Secrets via `Settings -> Secrets and variables -> Actions`:

* `OPENAI_API_KEY`: Your OpenAI API key from https://platform.openai.com/api-keys.
* `ALLOWED_USER`: Your exact GitHub handle (e.g., `OstinUA`).

## License

Distributed under the MIT License. See `LICENSE` for more information.

## Support the Project

If you find this tool useful, consider leaving a star on GitHub or supporting the author directly.
