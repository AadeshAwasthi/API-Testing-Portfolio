# API Test Cases: Restful-Booker

> Demo test cases for the public practice API https://restful-booker.herokuapp.com

**Total test cases:** 30

| TC ID | Module | Request | Test Case | Test Data | Expected Result | Type |
|---|---|---|---|---|---|---|
| API_TC_01 | Health Check | `GET /ping` | Verify API health check | - | 201 Created (as per API document) | Positive |
| API_TC_02 | Auth | `POST /auth` | Create token with valid credentials | admin / password123 | 200 OK; response has a non-empty `token` | Positive |
| API_TC_03 | Auth | `POST /auth` | Create token with wrong password | admin / wrong | 401 Unauthorized; no token is returned | Negative |
| API_TC_04 | Auth | `POST /auth` | Create token with wrong username | user1 / password123 | 401 Unauthorized; no token is returned | Negative |
| API_TC_05 | Auth | `POST /auth` | Create token with empty body | {} | 401 Unauthorized; no token is returned | Negative |
| API_TC_06 | Get Bookings | `GET /booking` | Get all booking IDs | - | 200 OK; array of objects with `bookingid` | Positive |
| API_TC_07 | Get Bookings | `GET /booking?firstname=&lastname=` | Filter bookings by name | firstname and lastname of an existing booking | 200 OK; list contains the ID of that booking | Positive |
| API_TC_08 | Get Bookings | `GET /booking?checkin=` | Filter bookings by check-in date | checkin=2026-12-01 | 200 OK; only bookings with check-in on or after the date | Positive |
| API_TC_09 | Get Bookings | `GET /booking?firstname=` | Filter with a name that does not exist | firstname=zzzzqa | 200 OK; empty array `[]` | Negative |
| API_TC_10 | Get Booking | `GET /booking/{id}` | Get booking by valid ID | Existing ID | 200 OK; all booking fields are returned | Positive |
| API_TC_11 | Get Booking | `GET /booking/{id}` | Get booking by ID that does not exist | 999999999 | 404 Not Found | Negative |
| API_TC_12 | Get Booking | `GET /booking/{id}` | Get booking with text as ID | abc | 404 Not Found | Negative |
| API_TC_13 | Get Booking | `GET /booking/{id}` | Verify response data types | Existing ID | `totalprice` is number, `depositpaid` is boolean, dates are YYYY-MM-DD | Positive |
| API_TC_14 | Create Booking | `POST /booking` | Create booking with all valid fields | Valid JSON body | 200 OK; `bookingid` is returned and `booking` matches the request | Positive |
| API_TC_15 | Create Booking | `POST /booking` | Verify created booking is saved | ID from previous test | GET returns the same data that was sent | Positive |
| API_TC_16 | Create Booking | `POST /booking` | Create booking without optional `additionalneeds` | Body without additionalneeds | 200 OK; booking is created | Positive |
| API_TC_17 | Create Booking | `POST /booking` | Create booking with missing mandatory fields | Only firstname | 400 Bad Request with a clear error message | Negative |
| API_TC_18 | Create Booking | `POST /booking` | Create booking with check-out before check-in | checkin 2026-12-10, checkout 2026-12-05 | 400 Bad Request; booking is not created | Negative |
| API_TC_19 | Create Booking | `POST /booking` | Create booking with negative price | totalprice: -100 | 400 Bad Request; booking is not created | Negative |
| API_TC_20 | Create Booking | `POST /booking` | Create booking with invalid date format | checkin: "10-12-2026" | 400 Bad Request; booking is not created | Negative |
| API_TC_21 | Update Booking | `PUT /booking/{id}` | Full update with valid token | All fields changed | 200 OK; response shows the updated values | Positive |
| API_TC_22 | Update Booking | `PUT /booking/{id}` | Full update without token | No Cookie header | 403 Forbidden; booking is not changed | Negative |
| API_TC_23 | Update Booking | `PUT /booking/{id}` | Full update with invalid token | Cookie: token=abc123 | 403 Forbidden; booking is not changed | Negative |
| API_TC_24 | Update Booking | `PATCH /booking/{id}` | Partial update of firstname and lastname | Two fields only | 200 OK; only the two fields are changed, other fields are same | Positive |
| API_TC_25 | Update Booking | `PATCH /booking/{id}` | Partial update without token | No Cookie header | 403 Forbidden | Negative |
| API_TC_26 | Delete Booking | `DELETE /booking/{id}` | Delete booking with valid token | Existing ID | 201 Created (as per API document) | Positive |
| API_TC_27 | Delete Booking | `GET /booking/{id}` | Verify deleted booking is removed | Deleted ID | 404 Not Found | Positive |
| API_TC_28 | Delete Booking | `DELETE /booking/{id}` | Delete booking without token | No Cookie header | 403 Forbidden; booking still exists | Negative |
| API_TC_29 | Delete Booking | `DELETE /booking/{id}` | Delete booking that does not exist | 999999999 | 404 Not Found | Negative |
| API_TC_30 | End to End | `All` | Create > Get > Update > Partial Update > Delete > Get | Chained requests | Each step passes and final GET returns 404 | E2E |

## Sample Request Body (Create Booking)

```json
{
  "firstname": "Aadesh",
  "lastname": "Awasthi",
  "totalprice": 1500,
  "depositpaid": true,
  "bookingdates": {
    "checkin": "2026-12-01",
    "checkout": "2026-12-05"
  },
  "additionalneeds": "Breakfast"
}
```

## Sample Response

```json
{
  "bookingid": 101,
  "booking": {
    "firstname": "Aadesh",
    "lastname": "Awasthi",
    "totalprice": 1500,
    "depositpaid": true,
    "bookingdates": { "checkin": "2026-12-01", "checkout": "2026-12-05" },
    "additionalneeds": "Breakfast"
  }
}
```

## Test Cases That Fail on the Demo API

| TC ID | Actual Result | Bug |
|---|---|---|
| API_TC_03, API_TC_04, API_TC_05 | `200 OK` with `{ "reason": "Bad credentials" }` instead of an error status code | API-BUG-01 |
| API_TC_17 | `500 Internal Server Error` | API-BUG-02 |
| API_TC_18 | `200 OK`, booking is created with check-out before check-in | API-BUG-03 |
| API_TC_19 | `200 OK`, booking is created with a negative price | API-BUG-07 |
| API_TC_20 | `200 OK`, dates are saved as wrong values | API-BUG-08 |
| API_TC_29 | `405 Method Not Allowed` instead of `404 Not Found` | API-BUG-05 |

API-BUG-04 and API-BUG-06 are status code design issues: the API returns `201 Created` for delete and for the health check, as written in its document.

Details are in [API Bug Reports](../bug-reports/API_Bug_Reports.md).
