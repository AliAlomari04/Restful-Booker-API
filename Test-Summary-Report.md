# Test Summary Report — Restful-Booker API

## 1. Executive Summary
This document summarizes the test execution results for the **Restful-Booker API**. Testing covered functional workflows, data validations, edge/boundary cases, and negative security scenarios across all major endpoints.

* **Execution Date:** November 2026
* **Execution Tools:** Postman Collection Runner, Newman CLI, GitHub Actions CI/CD
* **Environment:** Heroku Hosted Test Server (`https://restful-booker.herokuapp.com`)

---

## 2. Test Execution Metrics

| Metric | Result |
| :--- | :--- |
| **Total Test Requests Executed** | 16 |
| **Total Test Assertions** | 18 |
| **Passed Assertions** | 16 (88.9%) |
| **Failed Assertions** | 2 (11.1%) |
| **Script / Execution Errors** | 0 |
| **Average Response Time** | ~179 ms |

> **Note on Failures:** The 2 recorded failures are **verified API bugs** (server unexpected behavior against standard HTTP specifications), not test script errors.

---

## 3. Test Coverage by Endpoint

| Endpoint | Methods Covered | Scope of Validation | Status |
| :--- | :--- | :--- | :--- |
| `/ping` | `GET` | Health check & service availability | Passed |
| `/auth` | `POST` | Valid credential token generation | Passed |
| `/booking` | `POST` | Full creation, missing fields handling | 1 Defect Identified |
| `/booking/:id` | `GET` | By valid ID, JSON schema validation, non-existent ID | Passed |
| `/booking/:id` | `PUT` | Full update, auth enforcement, non-existent ID | 1 Defect Identified |
| `/booking/:id` | `PATCH` | Partial updates with valid tokens | Passed |
| `/booking/:id` | `DELETE` | Successful deletion & post-delete state verification | Passed |
| `/booking?filters` | `GET` | Query by name, date ranges, and non-existent queries | Passed |

---

## 4. Identified Defects Summary

1. **BUG-01 (Severity: High):** `POST /booking` returns `500 Internal Server Error` instead of `400 Bad Request` when required body fields are omitted.
2. **BUG-02 (Severity: Medium):** `PUT /booking/:id` on a non-existent booking ID returns `405 Method Not Allowed` instead of `404 Not Found`.

---

## 5. Limitations & Environment Constraints
* **Ephemeral Database (10-Minute Reset):** The public Restful-Booker server resets its state and wipes newly created data every 10 minutes.
* **Filter Consistency:** Query filtering on date ranges and names is subject to shared state modifications by other concurrent users.