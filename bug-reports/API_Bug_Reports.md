# API Bug Reports: Restful-Booker

> Demo bug reports on the public practice API https://restful-booker.herokuapp.com. This API is built for testers and contains intentional defects. Behaviour may change over time.

**Environment:** Postman, Base URL `https://restful-booker.herokuapp.com`  
**Reported By:** Aadesh Awasthi

| Bug ID | Title | Severity | Priority |
|---|---|---|---|
| API-BUG-01 | Invalid login returns 200 OK instead of 401 | Major | High |
| API-BUG-02 | Create booking with missing fields returns 500 Internal Server Error | Major | High |
| API-BUG-03 | Booking is created with check-out date before check-in date | Major | Medium |
| API-BUG-04 | Delete booking returns 201 Created | Minor | Medium |
| API-BUG-05 | Delete of a booking that does not exist returns 405 | Minor | Low |
| API-BUG-06 | Health check returns 201 Created instead of 200 OK | Minor | Low |

---

## API-BUG-01: Invalid login returns 200 OK instead of 401

**Request**

```
POST /auth
Content-Type: application/json

{ "username": "admin", "password": "wrong" }
```

**Expected Result:** `401 Unauthorized` with an error message.  
**Actual Result:** `200 OK` with body `{ "reason": "Bad credentials" }`.  
**Impact:** A client that checks only the status code will treat a failed login as success.

---

## API-BUG-02: Create booking with missing fields returns 500

**Request**

```
POST /booking
Content-Type: application/json

{ "firstname": "Aadesh" }
```

**Expected Result:** `400 Bad Request` with a message that lists the missing fields.  
**Actual Result:** `500 Internal Server Error`.  
**Impact:** Invalid input from the client is not validated and causes a server error.

---

## API-BUG-03: Booking is created with check-out before check-in

**Request**

```
POST /booking
Content-Type: application/json

{
  "firstname": "Aadesh", "lastname": "Awasthi", "totalprice": 1500, "depositpaid": true,
  "bookingdates": { "checkin": "2026-12-10", "checkout": "2026-12-05" },
  "additionalneeds": "Breakfast"
}
```

**Expected Result:** `400 Bad Request`; check-out date must be after check-in date.  
**Actual Result:** `200 OK`; booking is created with invalid dates.  
**Impact:** Invalid bookings can be saved in the system.

---

## API-BUG-04: Delete booking returns 201 Created

**Request**

```
DELETE /booking/{id}
Cookie: token=<valid token>
```

**Expected Result:** `200 OK` or `204 No Content`.  
**Actual Result:** `201 Created` with body `Created`.  
**Impact:** Status code is misleading; 201 means a new resource was created.

---

## API-BUG-05: Delete of a booking that does not exist returns 405

**Steps**

1. Delete a booking with a valid token.
2. Send the same DELETE request again.

**Expected Result:** `404 Not Found`.  
**Actual Result:** `405 Method Not Allowed`.  
**Impact:** Wrong status code; DELETE method is allowed on this endpoint.

---

## API-BUG-06: Health check returns 201 Created

**Request:** `GET /ping`

**Expected Result:** `200 OK`.  
**Actual Result:** `201 Created` with body `Created`.  
**Impact:** Low. A GET request does not create anything, so 201 is not the correct code. (The API document also mentions 201, so this is raised as a design issue.)
