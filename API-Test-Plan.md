# API Test Plan — Restful-Booker

## 1. Scope
### In-Scope
* Functional and non-functional API testing of all 8 endpoints in Restful-Booker.
* JSON schema validation , Authorization using the token key , Search with filtering

### Out-of-Scope
* UI / Frontend testing.
* Load / Stress testing

---

## 2. Target Endpoints
| Method | Endpoint | Description | Auth Required? |
| :--- | :--- | :--- | :--- |
| GET | `/ping` | Health check to verify service availability | No |
| POST | `/auth` | Generate auth token for update/delete operations | No |
| GET | `/booking` | Get all the booking IDs (filtering as well) | No |
| GET | `/booking/:id` | Get a specific booking information using its ID| No |
| POST | `/booking` | Create a new booking | No |
| PUT | `/booking/:id` | Update a current booking (multiple new information) | Yes (Cookie / Basic) |
| PATCH | `/booking/:id` | Update a specific booking information | Yes (Cookie / Basic) |
| DELETE | `/booking/:id` | Delete a booking that exists in the API | Yes (Cookie / Basic) |

---

## 3. Testing Types
* **Functional Testing:** Verifying that valid requests return expected responses and status codes (e.g., creating and retrieving a booking).
* **Negative Testing:** Verifying the API gracefully handles invalid inputs (e.g. missing required fields , invalid IDs) and returns appropriate 4xx client error codes.
* **Authorization Testing:** Verifying if the app has a valid error message if the token is missing in PUT/PATCH/DELETE endpoints.
* **Boundary Testing:** Verifying if the boundary fields have a valid messages if the request is out-of-boundary.
* **Data Validation:** Verifying JSON schema, field data types (`string`, `number`, `boolean`), and date format (`YYYY-MM-DD`).

---

## 4. Test Environment & Tools
* **Base URL:** `https://restful-booker.herokuapp.com`
* **Testing Tool:** Postman
* **CLI Runner:** Newman 
* **Data Format:** JSON

---

## 5. Assumptions
* The API resets its database every 10 minutes to its default state.
* The official API documentation is considered the source of truth (e.g., 201 Created is expected for GET /ping and DELETE /booking/:id as documented, even though it deviates from standard REST conventions).

---

## 6. Risks & Mitigation
* **Risk 1:** Public API shared with other users (test data might be modified or deleted by others during testing).
  * **Mitigation:** We make new booking and make the `bookingId` dynamically changed and not a static number.
* **Risk 2:** Intentional bugs exist in the API that contradict REST standards or documentation.
  * **Mitigation:** Document any mismatch between actual behavior and API documentation as a defect/bug.

---

## 7. Pass / Fail Criteria
* **Pass Criteria:** Correct status code , JSON Schema validation works correctly , Valid responses.
* **Fail Criteria:** Accepting Invalid/missing credentials , Return a `500 Internal server error` , .