# Restful-Booker API — Test Cases

## Coverage Summary

| Endpoint | HTTP Method | Total Test Cases | Functional | Negative / Auth / Boundary / Validation |
| :--- | :--- | :---: | :---: | :---: |
| `/ping` | `GET` | 1 | 1 | 0 |
| `/auth` | `POST` | 5 | 1 | 4 |
| `/booking` | `GET` | 7 | 6 | 1 |
| `/booking/:id` | `GET` | 4 | 1 | 3 |
| `/booking` | `POST` | 10 | 2 | 8 |
| `/booking/:id` | `PUT` | 6 | 1 | 5 |
| `/booking/:id` | `PATCH` | 4 | 2 | 2 |
| `/booking/:id` | `DELETE` | 4 | 2 | 2 |
| **Total** | — | **41** | **16** | **25** |

---

## 1. Health Check (`GET /ping`)

| Test Case ID | Endpoint | Test Type | Title | Preconditions | Test Steps | Test Data | Expected Result | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **API-PING-01** | `GET /ping` | Functional | Verify API health check endpoint returns service availability | API server is up and Base URL is configured in Postman environment. | 1. Select GET method.<br>2. Set URL to `{{baseUrl}}/ping`.<br>3. Send the request. | None (No parameters or body required) | Status Code: `201 Created`.<br>Response Body: Plain text `'Created'` indicating service is healthy. | Pass |

---

## 2. Authentication (`POST /auth`)

| Test Case ID | Endpoint | Test Type | Title | Preconditions | Test Steps | Test Data | Expected Result | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **API-AUTH-01** | `POST /auth` | Functional | Verify generating an auth token with valid username and password | API is running; `Content-Type` header is set to `application/json`. | 1. Select POST method.<br>2. Set URL to `{{baseUrl}}/auth`.<br>3. Add valid credentials in JSON body.<br>4. Send the request. | `{"username": "admin", "password": "password123"}` | Status Code: `200 OK`.<br>Response Body: Valid JSON containing a non-empty string property `'token'`. | Pass |
| **API-AUTH-02** | `POST /auth` | Negative | Verify login fails with valid username and invalid password | API is running; `Content-Type` header is set to `application/json`. | 1. Select POST method.<br>2. Set URL to `{{baseUrl}}/auth`.<br>3. Add credentials in JSON body.<br>4. Send the request. | `{"username": "admin", "password": "wrongpassword"}` | Status Code: `401 Unauthorized` (or `200 OK` with `{"reason": "Bad credentials"}` per API doc). | Pass |
| **API-AUTH-03** | `POST /auth` | Negative | Verify login fails with invalid username and valid password | API is running; `Content-Type` header is set to `application/json`. | 1. Select POST method.<br>2. Set URL to `{{baseUrl}}/auth`.<br>3. Add credentials in JSON body.<br>4. Send the request. | `{"username": "admin1", "password": "password123"}` | Status Code: `401 Unauthorized` (or `200 OK` with `{"reason": "Bad credentials"}` per API doc). | Pass |
| **API-AUTH-04** | `POST /auth` | Negative | Verify login fails when a required credential field is missing | API is running; `Content-Type` header is set to `application/json`. | 1. Select POST method.<br>2. Set URL to `{{baseUrl}}/auth`.<br>3. Add incomplete JSON body.<br>4. Send the request. | `{"username": "admin"}` | Status Code: `400 Bad Request` (or `200 OK` with `{"reason": "Bad credentials"}`). | Fail |
| **API-AUTH-05** | `POST /auth` | Negative | Verify login fails with an empty request body | API is running; `Content-Type` header is set to `application/json`. | 1. Select POST method.<br>2. Set URL to `{{baseUrl}}/auth`.<br>3. Send empty JSON body `{}`. | `{}` | Status Code: `400 Bad Request` (or `200 OK` with `{"reason": "Bad credentials"}`). | Pass |

---

## 3. Get Booking IDs (`GET /booking`)

| Test Case ID | Endpoint | Test Type | Title | Preconditions | Test Steps | Test Data | Expected Result | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **API-BOOKING-GET-01** | `GET /booking` | Functional | Verify get booking returns all the booking IDs without filters | API is running. | 1. Select GET method.<br>2. Set URL to `{{baseUrl}}/booking`.<br>3. Send the request. | None (No parameters or body required) | Status Code: `200 OK`.<br>Response Body: JSON array containing objects with `bookingid`. | Pass |
| **API-BOOKING-GET-02** | `GET /booking` | Functional | Verify get booking returns filtered booking IDs using the `firstname` filter | API is running; at least one booking with the target `firstname` exists. | 1. Select GET method.<br>2. Set URL to `{{baseUrl}}/booking?firstname=sally`.<br>3. Send the request. | `firstname` = `sally` | Status Code: `200 OK`.<br>Response Body: Array of booking IDs matching `firstname=sally`. | Not Run |
| **API-BOOKING-GET-03** | `GET /booking` | Functional | Verify get booking returns filtered booking IDs using the `lastname` filter | API is running; at least one booking with the target `lastname` exists. | 1. Select GET method.<br>2. Set URL to `{{baseUrl}}/booking?lastname=brown`.<br>3. Send the request. | `lastname` = `brown` | Status Code: `200 OK`.<br>Response Body: Array of booking IDs matching `lastname=brown`. | Not Run |
| **API-BOOKING-GET-04** | `GET /booking` | Functional | Verify get booking returns filtered booking IDs using both `firstname` and `lastname` filters | API is running; a matching booking exists. | 1. Select GET method.<br>2. Set URL to `{{baseUrl}}/booking?firstname=sally&lastname=brown`.<br>3. Send the request. | `firstname` = `sally`<br>`lastname` = `brown` | Status Code: `200 OK`.<br>Response Body: Array of booking IDs matching both criteria. | Not Run |
| **API-BOOKING-GET-05** | `GET /booking` | Functional | Verify get booking returns booking IDs using the `checkin` date filter | API is running. | 1. Select GET method.<br>2. Set URL to `{{baseUrl}}/booking?checkin=2014-03-13`.<br>3. Send the request. | `checkin` = `2014-03-13` | Status Code: `200 OK`.<br>Response Body: Array of bookings with checkin date `>= 2014-03-13`. | Not Run |
| **API-BOOKING-GET-06** | `GET /booking` | Functional | Verify get booking returns booking IDs using the `checkout` date filter | API is running. | 1. Select GET method.<br>2. Set URL to `{{baseUrl}}/booking?checkout=2014-05-21`.<br>3. Send the request. | `checkout` = `2014-05-21` | Status Code: `200 OK`.<br>Response Body: Array of bookings with checkout date `>= 2014-05-21`. | Not Run |
| **API-BOOKING-GET-07** | `GET /booking` | Negative | Verify get booking returns an empty array for a non-existent `firstname` | API is running. | 1. Select GET method.<br>2. Set URL to `{{baseUrl}}/booking?firstname=xyznonexistent`.<br>3. Send the request. | `firstname` = `xyznonexistent` | Status Code: `200 OK`.<br>Response Body: Empty JSON array `[]`. | Not Run |

---

## 4. Get Booking by ID (`GET /booking/:id`)

| Test Case ID | Endpoint | Test Type | Title | Preconditions | Test Steps | Test Data | Expected Result | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **API-BOOKING-ID-01** | `GET /booking/:id` | Functional | Verify getting a booking by a valid existing ID | A valid booking exists and its ID is known. | 1. Select GET.<br>2. Set URL to `{{baseUrl}}/booking/{{bookingId}}`.<br>3. Send request. | `bookingId` = existing ID | Status Code: `200 OK`.<br>Response Body: Contains the exact booking details. | Not Run |
| **API-BOOKING-ID-02** | `GET /booking/:id` | Data Validation | Verify the JSON schema, data types, and headers for a valid booking response | A valid booking exists and its ID is known. | 1. Select GET.<br>2. Set URL to `{{baseUrl}}/booking/{{bookingId}}`.<br>3. Send request. | `bookingId` = existing ID | Status Code: `200 OK`.<br>Headers: `Content-Type` includes `application/json`.<br>Body matches expected JSON schema. | Not Run |
| **API-BOOKING-ID-03** | `GET /booking/:id` | Negative | Verify getting a booking by a non-existent ID returns an error | API is running. | 1. Select GET.<br>2. Set URL to `{{baseUrl}}/booking/999999`.<br>3. Send request. | `bookingId` = `999999` | Status Code: `404 Not Found`.<br>Response Body: `"Not Found"`. | Not Run |
| **API-BOOKING-ID-04** | `GET /booking/:id` | Negative | Verify getting a booking with an alphabetical ID returns an error | API is running. | 1. Select GET.<br>2. Set URL to `{{baseUrl}}/booking/abc`.<br>3. Send request. | `bookingId` = `abc` | Status Code: `404 Not Found` (or `400 Bad Request`). | Not Run |

---

## 5. Create Booking (`POST /booking`)

| Test Case ID | Endpoint | Test Type | Title | Preconditions | Test Steps | Test Data | Expected Result | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **API-BOOKING-POST-01** | `POST /booking` | Functional | Verify creating a new booking with all valid required and optional fields | API is running. | 1. Select POST.<br>2. Set URL to `{{baseUrl}}/booking`.<br>3. Add full JSON body.<br>4. Send request. | Valid JSON with all 6 fields (including `additionalneeds`). | Status Code: `200 OK`.<br>Response Body: Contains a new `bookingid` and the `booking` object. | Not Run |
| **API-BOOKING-POST-02** | `POST /booking` | Functional | Verify creating a booking without the optional `additionalneeds` field | API is running. | 1. Select POST.<br>2. Set URL to `{{baseUrl}}/booking`.<br>3. Add JSON body.<br>4. Send request. | Valid JSON excluding the `additionalneeds` field. | Status Code: `200 OK`.<br>Response Body: Contains a new `bookingid` without `additionalneeds`. | Not Run |
| **API-BOOKING-POST-03** | `POST /booking` | Negative | Verify creating a booking with a missing mandatory field | API is running. | 1. Select POST.<br>2. Set URL to `{{baseUrl}}/booking`.<br>3. Add JSON body.<br>4. Send request. | JSON missing the `firstname` field. | Status Code: `400 Bad Request` (or `500 Internal Server Error` per actual API behavior). | Not Run |
| **API-BOOKING-POST-04** | `POST /booking` | Negative | Verify creating a booking with the entire `bookingdates` object missing | API is running. | 1. Select POST.<br>2. Set URL to `{{baseUrl}}/booking`.<br>3. Add JSON body.<br>4. Send request. | JSON missing the `bookingdates` object completely. | Status Code: `400 Bad Request`. | Not Run |
| **API-BOOKING-POST-05** | `POST /booking` | Data Validation | Verify creating a booking with an invalid date format | API is running. | 1. Select POST.<br>2. Set URL to `{{baseUrl}}/booking`.<br>3. Add JSON body.<br>4. Send request. | `checkin` = `"15-11-2026"` (`DD-MM-YYYY` instead of `YYYY-MM-DD`). | Status Code: `400 Bad Request`. | Not Run |
| **API-BOOKING-POST-06** | `POST /booking` | Boundary | Verify creating a booking with a negative total price | API is running. | 1. Select POST.<br>2. Set URL to `{{baseUrl}}/booking`.<br>3. Add JSON body.<br>4. Send request. | `totalprice` = `-150` | Status Code: `400 Bad Request`. | Not Run |
| **API-BOOKING-POST-07** | `POST /booking` | Data Validation | Verify creating a booking with a string value for total price | API is running. | 1. Select POST.<br>2. Set URL to `{{baseUrl}}/booking`.<br>3. Add JSON body.<br>4. Send request. | `totalprice` = `"one hundred"` | Status Code: `400 Bad Request`. | Not Run |
| **API-BOOKING-POST-08** | `POST /booking` | Boundary | Verify creating a booking with an excessively long `firstname` | API is running. | 1. Select POST.<br>2. Set URL to `{{baseUrl}}/booking`.<br>3. Add JSON body.<br>4. Send request. | `firstname` = `[String of 500 characters]` | Status Code: `400 Bad Request`. | Not Run |
| **API-BOOKING-POST-09** | `POST /booking` | Data Validation | Verify creating a booking with special characters or HTML tags in `firstname` | API is running. | 1. Select POST.<br>2. Set URL to `{{baseUrl}}/booking`.<br>3. Add JSON body.<br>4. Send request. | `firstname` = `"<script>alert(1)</script>"` | Status Code: `400 Bad Request` (to prevent XSS). | Not Run |
| **API-BOOKING-POST-10** | `POST /booking` | Negative | Verify creating a booking with a `checkout` date earlier than `checkin` date | API is running. | 1. Select POST.<br>2. Set URL to `{{baseUrl}}/booking`.<br>3. Add JSON body.<br>4. Send request. | `checkin` = `"2026-11-20"`<br>`checkout` = `"2026-11-10"` | Status Code: `400 Bad Request` (Logical validation failure). | Not Run |

---

## 6. Update Booking (`PUT /booking/:id`)

| Test Case ID | Endpoint | Test Type | Title | Preconditions | Test Steps | Test Data | Expected Result | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **API-BOOKING-PUT-01** | `PUT /booking/:id` | Functional | Verify updating an entire booking successfully using a valid Cookie token | A valid booking exists. Token is generated. | 1. Select PUT.<br>2. URL: `{{baseUrl}}/booking/{{bookingId}}`.<br>3. Add `Cookie: token={{token}}` header.<br>4. Add full JSON body.<br>5. Send request. | `firstname` = `"Ali_Updated"` plus all other required fields. | Status Code: `200 OK`.<br>Response Body: Contains the updated booking details. | Not Run |
| **API-BOOKING-PUT-02** | `PUT /booking/:id` | Authorization | Verify updating a booking successfully using Basic Auth header | A valid booking exists. | 1. Select PUT.<br>2. URL: `{{baseUrl}}/booking/{{bookingId}}`.<br>3. Add `Authorization: Basic YWRtaW46cGFzc3dvcmQxMjM=`.<br>4. Add full JSON body.<br>5. Send request. | Full valid booking JSON body. | Status Code: `200 OK`.<br>Response Body: Contains the updated booking details. | Not Run |
| **API-BOOKING-PUT-03** | `PUT /booking/:id` | Authorization | Verify updating a booking fails when no Authorization/Cookie token is provided | A valid booking exists. | 1. Select PUT.<br>2. URL: `{{baseUrl}}/booking/{{bookingId}}`.<br>3. Do not add token headers.<br>4. Send request. | Valid JSON body. | Status Code: `403 Forbidden`.<br>Response Body: `"Forbidden"`. | Not Run |
| **API-BOOKING-PUT-04** | `PUT /booking/:id` | Authorization | Verify updating a booking fails when an invalid token is provided | A valid booking exists. | 1. Select PUT.<br>2. Add header `Cookie: token=invalidToken123`.<br>3. Send request. | Valid JSON body. | Status Code: `403 Forbidden`. | Not Run |
| **API-BOOKING-PUT-05** | `PUT /booking/:id` | Negative | Verify updating a booking fails for a non-existent booking ID | Token is generated. | 1. Select PUT.<br>2. URL: `{{baseUrl}}/booking/999999`.<br>3. Add valid token header.<br>4. Send request. | Valid JSON body. | Status Code: `405 Method Not Allowed` (or `404 Not Found`). | Not Run |
| **API-BOOKING-PUT-06** | `PUT /booking/:id` | Negative | Verify updating a booking fails when a mandatory field is missing in the body | A valid booking exists. Token is generated. | 1. Select PUT.<br>2. URL: `{{baseUrl}}/booking/{{bookingId}}`.<br>3. Add valid token header.<br>4. Send request. | JSON missing the `totalprice` field. | Status Code: `400 Bad Request`. | Not Run |

---

## 7. Partial Update Booking (`PATCH /booking/:id`)

| Test Case ID | Endpoint | Test Type | Title | Preconditions | Test Steps | Test Data | Expected Result | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **API-BOOKING-PATCH-01** | `PATCH /booking/:id` | Functional | Verify partially updating only the `firstname` and `lastname` | A valid booking exists. Token is generated. | 1. Select PATCH.<br>2. Add Cookie token header.<br>3. Add JSON body with 2 fields.<br>4. Send request. | `{"firstname": "James", "lastname": "Smith"}` | Status Code: `200 OK`.<br>Body: Booking updated with new names, other fields remain unchanged. | Not Run |
| **API-BOOKING-PATCH-02** | `PATCH /booking/:id` | Functional | Verify partially updating only the `bookingdates` | A valid booking exists. Token is generated. | 1. Select PATCH.<br>2. Add Cookie token header.<br>3. Add JSON body.<br>4. Send request. | `{"bookingdates": {"checkin": "2027-01-01", "checkout": "2027-01-10"}}` | Status Code: `200 OK`.<br>Body: Dates are updated correctly. | Not Run |
| **API-BOOKING-PATCH-03** | `PATCH /booking/:id` | Negative | Verify partially updating a booking with an unknown field | A valid booking exists. Token is generated. | 1. Select PATCH.<br>2. Add Cookie token header.<br>3. Add JSON body.<br>4. Send request. | `{"middleName": "Ali"}` | Status Code: `400 Bad Request` (or `200 OK` ignoring the unknown field, depending on API design). | Not Run |
| **API-BOOKING-PATCH-04** | `PATCH /booking/:id` | Authorization | Verify partially updating a booking fails without an Auth token | A valid booking exists. | 1. Select PATCH.<br>2. Do not add token headers.<br>3. Send request. | `{"firstname": "Hacked"}` | Status Code: `403 Forbidden`. | Not Run |

---

## 8. Delete Booking (`DELETE /booking/:id`)

| Test Case ID | Endpoint | Test Type | Title | Preconditions | Test Steps | Test Data | Expected Result | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **API-BOOKING-DEL-01** | `DELETE /booking/:id` | Functional | Verify deleting an existing booking successfully with a valid token | A valid booking exists. Token is generated. | 1. Select DELETE.<br>2. URL: `{{baseUrl}}/booking/{{bookingId}}`.<br>3. Add Cookie token header.<br>4. Send request. | None | Status Code: `201 Created` (As per API documentation). | Not Run |
| **API-BOOKING-DEL-02** | `DELETE /booking/:id` | Authorization | Verify deleting a booking fails when no Auth token is provided | A valid booking exists. | 1. Select DELETE.<br>2. URL: `{{baseUrl}}/booking/{{bookingId}}`.<br>3. Do not add token headers.<br>4. Send request. | None | Status Code: `403 Forbidden`. | Not Run |
| **API-BOOKING-DEL-03** | `DELETE /booking/:id` | Functional | Verify the booking is actually removed from the system after successful deletion | Booking was deleted in the previous step. | 1. Select GET.<br>2. URL: `{{baseUrl}}/booking/{{bookingId}}`.<br>3. Send request. | `bookingId` = ID of the deleted booking | Status Code: `404 Not Found`.<br>Response Body: `"Not Found"`. | Not Run |
| **API-BOOKING-DEL-04** | `DELETE /booking/:id` | Negative | Verify deleting the same booking twice returns an error (Double Delete) | Booking was deleted in the previous step. Token is generated. | 1. Select DELETE.<br>2. URL: `{{baseUrl}}/booking/{{bookingId}}`.<br>3. Add Cookie token header.<br>4. Send request. | `bookingId` = ID of the deleted booking | Status Code: `405 Method Not Allowed` (or `404 Not Found`). | Not Run |