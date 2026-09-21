# BUG-003: Empty Email Causes Internal Server Error

## Summary

The Send Verification Code endpoint returns an Internal Server Error when an empty email is provided instead of returning a proper validation error.

---

## Environment

- **API**: `http://mytherapy.runasp.net`
- **Endpoint**: `POST /api/auth/send-verification-code`
- **Role used**: Public / Unauthenticated
- **Authentication**: None

---

## Preconditions

1. A therapist account exists.
2. The therapist is authenticated with a valid therapist token.
3. The therapist already has an availability slot:
   `14:00 → 15:00`

---

## Steps to Reproduce

1. The API is running and accessible.
2. No authentication is required for the Send Verification Code endpoint.
3. The user sends a request with an empty email value.

```json
{
  "email": ""
}
```

---

## Expected Result

The API should validate the email input before attempting to send the verification email.

Expected HTTP status:

- `400 Bad Request`

The response should contain a clear validation error indicating that the email is required or invalid.

For Example:

```text

The Email field is required.

```

The API should not attempt to send an email when the email value is empty.

---

## Actual Result

- HTTP status: `500 Internal Server Error`
- The API returns an SMTP-related error.
- The response exposes internal email service details.

Actual response:

```json
{
  "StatusCode": 500,
  "Message": "5.5.2 Syntax error, cannot decode response. For more information, go to\n5.5.2 https://support.google.com/a/answer/3221692 and review RFC 5321\n5.5.2 specifications. 5b1f17b1804b1-49fd8ba0123sm1337055e9.4 - gsmtp"
}
```

---

## Impact

This is an input-validation and error-handling issue.

Invalid user input should not result in a `500 Internal Server Error`.

The current behavior may:

- Expose internal SMTP/email service details to the client.
- Make the API appear unstable when invalid input is provided.
- Make it harder for the frontend to display a proper validation message.
- Cause unnecessary attempts to communicate with the email service using invalid input.

---

## Suggested Severity

**Medium** — pending confirmation of the application's business impact and whether the exposed SMTP details are considered sensitive in the production environment.

---

## Regression Test

Keep an automated test that verifies the API rejects an empty email with a validation error instead of returning `500 Internal Server Error`.

The test should currently fail because the API returns `500`.

Example Postman assertions:

```javascript
pm.test("Empty email should return 400", function () {
  pm.response.to.have.status(400);
});
```

After the defect is fixed, the test should pass with:

```text
Expected: 400
Actual: 400
Result: PASS
```

---

## Notes

The exact validation message should follow the project's API contract.

1. An empty email must not be processed as a valid email address.
2. The API must return a client-side validation error such as `400 Bad Request`.
3. The API must not return `500 Internal Server Error` for invalid user input.
4. Internal SMTP/provider error details should not be exposed to the client.

This issue was discovered while testing the negative case:

```json
{
  "email": ""
}
```
