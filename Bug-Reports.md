# Bug Reports — Restful-Booker API

## BUG-01: POST /booking returns 500 Internal Server Error when required fields are missing

* **Bug Priority:** High
* **Severity:** High
* **Endpoint:** `POST https://restful-booker.herokuapp.com/booking`
* **Summary:** Sending a `POST` request to `/booking` with missing mandatory fields (omitting `firstname` and `totalprice`) causes an unhandled server exception instead of returning a client validation error.
* **Steps to Reproduce:**
  1. Set request method to `POST` and endpoint to `https://restful-booker.herokuapp.com/booking`.
  2. Set Headers: `Content-Type: application/json`, `Accept: application/json`.
  3. Send the following partial JSON body:
     ```json
     {
       "lastname": "Brown",
       "depositpaid": true,
       "bookingdates": {
         "checkin": "2026-11-10",
         "checkout": "2026-11-15"
       },
       "additionalneeds": "Breakfast"
     }
     ```
* **Expected Result:** Server validates the payload and returns `400 Bad Request`.
* **Actual Result:** Server crashes and returns `500 Internal Server Error`.


---

## BUG-02: PUT /booking/:id returns 405 Method Not Allowed for non-existent booking ID

* **Bug Priority:** Medium
* **Severity:** Medium
* **Endpoint:** `PUT https://restful-booker.herokuapp.com/booking/999999999`
* **Summary:** Attempting to update a non-existent booking ID using `PUT` returns `405 Method Not Allowed` instead of `404 Not Found`.
* **Steps to Reproduce:**
  1. Generate a valid authentication token via `POST /auth`.
  2. Send a `PUT` request to `https://restful-booker.herokuapp.com/booking/999999999` with header `Cookie: token=<valid_token>` and a complete valid booking JSON body.
* **Expected Result:** Server returns `404 Not Found` because the target booking resource does not exist.
* **Actual Result:** Server returns `405 Method Not Allowed`.
