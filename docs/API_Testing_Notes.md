# API Testing Notes

Short notes I use while testing REST APIs.

## HTTP Methods

| Method | Use | Example |
|---|---|---|
| GET | Read data | `GET /booking/1` |
| POST | Create data | `POST /booking` |
| PUT | Update the full record | `PUT /booking/1` |
| PATCH | Update part of the record | `PATCH /booking/1` |
| DELETE | Delete data | `DELETE /booking/1` |

## Common Status Codes

| Code | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request (invalid input) |
| 401 | Unauthorized (not logged in or invalid token) |
| 403 | Forbidden (no permission) |
| 404 | Not Found |
| 405 | Method Not Allowed |
| 500 | Internal Server Error |

## API Testing Checklist

- [ ] Correct status code for valid request
- [ ] Response body has all expected fields with correct values
- [ ] Data types are correct (string, number, boolean)
- [ ] Mandatory field validation (missing field gives 400)
- [ ] Invalid data validation (wrong type, wrong format, negative values)
- [ ] Authentication: request without token and with invalid token is rejected
- [ ] Invalid or non-existing ID gives 404
- [ ] Data is really saved in the system (verify with GET after POST, PUT, PATCH, DELETE)
- [ ] Response time is acceptable
- [ ] Error messages are clear and do not show server details

## Useful Postman Test Snippets

```javascript
// Status code
pm.test("Status code is 200", () => pm.response.to.have.status(200));

// Field value
pm.test("First name is correct", () => pm.expect(pm.response.json().firstname).to.eql("Aadesh"));

// Data type
pm.test("Price is a number", () => pm.expect(pm.response.json().totalprice).to.be.a("number"));

// Response time
pm.test("Response time below 2 seconds", () => pm.expect(pm.response.responseTime).to.be.below(2000));

// Save a value for the next request
pm.collectionVariables.set("bookingId", pm.response.json().bookingid);
```
