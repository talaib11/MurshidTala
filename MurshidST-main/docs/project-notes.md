# Murshid Mini — Project Notes

**Course:** LLM Application Engineering (SDA-AIE-213)  
**Project:** MurshidST  
**Contributors:** Shoog Algasim and Tala Alshowiman  
**Implementation:** Python and Google Colab

## 1. Project Overview

Murshid Mini is an educational application developed to demonstrate the main engineering components of an LLM-style service assistant.

The application supports Arabic and English service-related questions and simulated application-status inquiries. Its core implementation uses a FakeClient, predefined responses, and synthetic records without requiring external language model API calls.

The project was developed incrementally across four labs, covering application foundations, tool integration, safety guardrails, evaluation, caching, and reporting.

## 2. System Design and Architecture

The application consists of the following main components:

### 2.1 Input Processing

The system receives user questions and performs input validation. Invalid or unsupported requests are rejected with appropriate response codes.

### 2.2 FakeClient

The FakeClient simulates a language model by returning predefined responses. It provides a controlled environment for testing application behavior without connecting to an external model.

### 2.3 Request Routing

The application distinguishes between general service questions and application-status inquiries.

General service questions are answered using predefined service information, while application-status requests are processed through a structured workflow.

### 2.4 Tool Integration and Authorization

The system uses a simulated read-only tool to retrieve application-status information from synthetic records.

Before returning protected information, the workflow checks whether the requesting user is authorized to access the record.

Requests involving missing references, nonexistent records, or unauthorized access receive appropriate responses.

### 2.5 Safety Guardrails

Input validation and safety checks are used to handle invalid requests, unsupported actions, and attempts to override application instructions.

These controls are educational demonstrations and are not a substitute for production security mechanisms.

### 2.6 Caching

The system implements exact-match caching for supported public FAQ questions.

Application-status requests are excluded from the shared cache to prevent private information from being reused across users.

Cache keys incorporate version information so changes to the relevant prompt or guide version do not reuse outdated cached answers.

### 2.7 Metrics and Reporting

The application records execution information, including trace identifiers, response codes, cache hits, mock-client calls, tool calls, and measured Python execution time.

Results are exported as JSON files for evaluation and review.

## 3. Synthetic Data Documentation

The project uses the `synthetic-v1` dataset.

The dataset contains fictional application records used to demonstrate status retrieval, authorization checks, and safe error handling.

The test scenarios include:

- `REF-1001`: an accessible application with status `under_review`.
- `REF-9999`: a nonexistent application reference.
- `REF-1002`: an application that the demonstration user is not authorized to access.

The application also uses predefined Arabic and English FAQ responses.

All application records and demonstration scenarios are synthetic. No real government application records, national identification numbers, or personal service data are required.

Synthetic data makes the application safe to demonstrate and provides predictable test cases.

## 4. Implementation Stages

### Day 1 — Application Foundations

The first lab introduced the FakeClient, structured response objects, input validation, and baseline testing.

The baseline results were saved in `baseline_results.json`.

### Day 2 — Structured Tool Calls

The second lab introduced structured application-status requests, read-only tool execution, and authorization handling.

Five application scenarios were tested, covering successful status retrieval, clarification, missing records, unsupported actions, and access denial.

Results were saved in `day2_results.json`.

### Day 3 — Guardrails and Golden Testing

The third lab introduced additional safety checks and a golden test suite.

The suite covered ten cases, including valid service questions, application statuses, invalid inputs, unauthorized access, and blocked instructions.

All ten test cases passed.

### Day 4 — Caching and Final Evaluation

The fourth lab introduced exact-match caching, performance measurements, regression checks, and final reporting.

The caching experiment compared three repeated FAQ requests with and without caching.

The project also exported its evaluation results and generated reports.

## 5. How to Run the Project

1. Open `notebooks/MurshidST.ipynb` in Google Colab.
2. Select a Python 3 runtime using CPU.
3. Run all 14 code cells sequentially.
4. Review the outputs from each lab.
5. Confirm that the golden test suite passes.
6. Review the cache experiment and performance metrics.
7. Access the generated JSON files from the `murshid_outputs` directory.

The core project does not require a paid API key, external model inference, or GPU acceleration.

The optional export step creates `murshid_submission.zip`, containing generated JSON reports. The notebook must be saved separately.

## 6. Testing and Evaluation

### 6.1 Golden Test Suite

The final regression evaluation produced the following results:

| Metric | Result |
|---|---|
| Total Test Cases | 10 |
| Passed Test Cases | 10 |
| Pass Rate | 100% |
| Execution Mode | Mock |
| External Model API Calls | 0 |

The tests cover supported requests, application-status handling, authorization, invalid input, and safety checks.

### 6.2 Caching Experiment

| Metric | Without Cache | With Cache |
|---|---|---|
| Total Requests | 3 | 3 |
| Mock-Client Calls | 3 | 1 |
| Cache Hits | 0 | 2 |
| Cache Hit Rate | 0% | 66.7% |

The cache-hit sequence was:

`[false, true, true]`

The experiment demonstrates that caching reduces repeated mock-client calls for identical eligible requests.

These results represent a controlled simulation and do not establish real-world latency improvements or financial savings.

### 6.3 Execution Metrics

The generated `metrics.json` includes 22 execution records from the project experiments and tests.

Each record contains a trace identifier, execution mode, response code, cache-hit status, mock-call count, tool-call count, and execution latency measured in milliseconds.

The recorded execution times represent local Python execution in a mock environment rather than real external model inference latency.

### 6.4 Hypothetical Cost

The project includes an illustrative API cost calculation based on:

- 1,000 input tokens at $2 per million tokens.
- 400 output tokens at $4 per million tokens.

The estimated cost per request is:

**$0.0036 USD**

This calculation is hypothetical. The actual external model API cost for the core project was zero.

## 7. Generated Output Files

The core project generates the following reports:

| File | Description |
|---|---|
| `baseline_results.json` | Baseline application responses |
| `day2_results.json` | Structured tool-call test results |
| `day3_results.json` | Golden test suite results |
| `day4_results.json` | Day 4 experiment results |
| `metrics.json` | Execution and caching measurements |
| `final_report.json` | Consolidated evaluation report |
| `synthetic_data.json` | Synthetic demonstration data |

These files provide evidence of project execution and testing.

## 8. Project Limitations

The application has several limitations:

- It uses a simulated model rather than a live language model.
- The records and test cases are synthetic.
- Real user authentication is not implemented.
- Request routing is limited to predefined rules.
- Safety guardrails are educational demonstrations.
- The system has not been validated for production deployment.

The test results should not be interpreted as proof of production-level performance, reliability, or security.

## 9. Future Improvements

Potential improvements include integrating a real language model, introducing secure authentication, expanding the evaluation dataset, implementing stronger security controls, and conducting broader performance and semantic evaluation.

## 10. Training Attribution

This project was developed as part of the **LLM Application Engineering (SDA-AIE-213)** training program.

**Contributors:** Shoog Algasim and Tala Alshowiman

Acknowledgment is given to **SDAIA Academy**, the **Data and Artificial Intelligence Center of Excellence (DAICO)** at King Saud University, and instructor **Meaad Al-Marri** for their contributions to the training experience.

Original course materials:

https://github.com/almiyead-rgb/murshid-llm-application-engineering-sda-aie-213

This repository documents the contributors' implementation and learning outcomes based on the course materials.
