# API Testing Portfolio

![API](https://img.shields.io/badge/API-REST-blue) ![Postman](https://img.shields.io/badge/Postman-Collection-FF6C37?logo=postman&logoColor=white) ![Status](https://img.shields.io/badge/Project-Demo-orange)

API testing documentation prepared by **Aadesh Awasthi** (QA Engineer): test cases, a Postman collection with test scripts, and API bug reports.

> **Note:** This is a **demo project** on [Restful-Booker](https://restful-booker.herokuapp.com/apidoc/index.html), a public practice API for hotel bookings created for testers. It contains some intentional defects. No company or client API is used here.

## API Under Test

| Item | Details |
|---|---|
| API | Restful-Booker |
| Base URL | `https://restful-booker.herokuapp.com` |
| Type | REST, JSON |
| Auth | Token (sent as `Cookie: token=<value>`) for update and delete |

## Endpoints Covered

| Method | Endpoint | Purpose | Auth |
|---|---|---|---|
| GET | `/ping` | Health check | No |
| POST | `/auth` | Create token | No |
| GET | `/booking` | Get all booking IDs (with filters) | No |
| GET | `/booking/{id}` | Get one booking | No |
| POST | `/booking` | Create booking | No |
| PUT | `/booking/{id}` | Full update | Yes |
| PATCH | `/booking/{id}` | Partial update | Yes |
| DELETE | `/booking/{id}` | Delete booking | Yes |

## Repository Content

| File | Description |
|---|---|
| [API Test Cases](test-cases/API_Test_Cases.md) | 30 test cases: positive, negative, auth and end-to-end |
| [Postman Collection](postman/Restful_Booker.postman_collection.json) | Ready-to-import collection with test scripts |
| [API Bug Reports](bug-reports/API_Bug_Reports.md) | 6 defects found in the demo API |
| [API Testing Notes](docs/API_Testing_Notes.md) | HTTP methods, status codes and what to verify in API testing |

## How to Run the Postman Collection

1. Install [Postman](https://www.postman.com/downloads/).
2. Click **Import** and select `postman/Restful_Booker.postman_collection.json`.
3. Open the collection and click **Run collection**.
4. Requests run in order: token is created first, then a booking is created, read, updated and deleted. The token and booking ID are saved in collection variables automatically.

Run from the command line with Newman:

```bash
npm install -g newman
newman run postman/Restful_Booker.postman_collection.json
```

## What I Verify in API Testing

- Status code
- Response body fields and values
- Data types and mandatory fields
- Response time
- Authentication and authorization
- Negative cases: invalid data, missing fields, wrong ID
- Data is really saved, updated or deleted (verify with a GET call)
