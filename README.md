# MyTherapy API Testing Project

## Overview

This project demonstrates API testing for a healthcare/therapy backend application built with ASP.NET Core.

The testing was performed using Postman and covers authentication, email verification, therapist availability, booking, authorization, and negative testing scenarios.

---

## Tools & Technologies

- Postman
- REST API
- JSON
- JavaScript
- ASP.NET Core Web API
- SQL Server
- Git & GitHub

---

## API Testing Scope

The project currently covers:

### Authentication

- Patient Login
- Therapist Login
- Admin Login
- Invalid credentials
- Missing required fields
- Invalid email format
- Empty request body
- Null values

### Email Verification

- Send verification code
- Valid email
- Missing email
- Empty email
- Null email
- Invalid email format
- Whitespace email

### Therapist Availability

- Create availability slot
- Invalid time range
- Past availability
- Overlapping availability
- Delete availability
- Unauthorized access
- Cross-user authorization testing

### Patient Booking

- Get available slots
- Book an appointment
- Verify booked slot
- Retrieve patient's bookings
- Duplicate booking

---

## Defects Found

During testing, multiple issues were identified and documented.

| ID      | Area               | Description                                           | Severity |
| ------- | ------------------ | ----------------------------------------------------- | -------- |
| BUG-001 | Availability       | Overlapping availability can be created               | Medium   |
| BUG-002 | Authorization      | Therapist can delete another therapist's availability | High     |
| BUG-003 | Email Verification | Invalid email input causes HTTP 500                   | Medium   |

Detailed reports are available in the [`bug-reports`](./bug-reports) directory.

---

## Postman Collection

The Postman collection contains the API requests and automated assertions used during testing.

Import the collection from:

`postman/MyTherapy-API-Collection.json`

The environment file is provided as a sanitized template.

> Authentication tokens and sensitive credentials are not included in the repository.

---

## Testing Approach

The project uses both positive and negative testing.

### Positive Testing

Valid requests are used to verify that the API behaves correctly for expected user actions.

### Negative Testing

Invalid inputs, missing fields, unauthorized requests, duplicate operations, and boundary conditions are tested to verify proper error handling and API validation.

### Authorization Testing

Different user roles were used to verify access control between:

- Patient
- Therapist
- Admin

---

## Example Postman Assertion

```javascript
pm.test("Status code is 200", function () {
  pm.response.to.have.status(200);
});
```

---

## Project Structure

```text
mytherapy-api-testing/
│
├── README.md
├── postman/
│   ├── MyTherapy-API-Collection.json
│   └── MyTherapy-API-Environment-Sanitized.json
│
├── bug-reports/
│   ├── BUG-001-overlapping-availability.md
│   ├── BUG-002-cross-therapist-delete.md
│   └── BUG-003-invalid-email-input.md
│
└── docs/
    └── test-summary.md
```
