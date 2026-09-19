# BUG-001: Therapist Can Create Overlapping Availability Slots

## Summary

A therapist is able to create an availability slot that overlaps with an existing availability slot for the same therapist.

---

## Environment

- **API**: `http://mytherapy.runasp.net`
- **Endpoint**: `POST /api/therapist/availability`
- **Role used**: Therapist
- **Authentication**: Bearer token

---

## Preconditions

1. A therapist account exists.
2. The therapist is authenticated with a valid therapist token.
3. The therapist already has an availability slot:
   `14:00 → 15:00`

---

## Steps to Reproduce

1. Log in as a Therapist.
2. Create an availability slot from `14:00` to `15:00`.
3. Send another `POST` request to:
   `POST /api/therapist/availability`
4. Use an overlapping time range:

```json
{
  "startTime": "2026-09-17T14:30:00Z",
  "endTime": "2026-09-17T15:30:00Z"
}
```

---

## Expected Result

The API should prevent the therapist from creating an availability slot that overlaps with an existing availability slot.

Expected HTTP status:

- `400 Bad Request`

Expected error message should indicate that the requested availability overlaps with an existing slot.

For Example:

```
Availability overlaps with an existing slot.
```

---

## Actual Result

- HTTP status: `200 OK`
- The API successfully creates the overlapping availability slot.
- No validation error is returned.

---

## Impact

This is a business-logic validation issue.
Allowing overlapping availability slots for the same therapist can result in conflicting availability periods and may cause incorrect scheduling or booking behavior.

---

## Suggested Severity

**Medium** — pending confirmation of the application's business impact and production exposure.

---

## Regression Test

Keep an automated test that verifies a therapist cannot create an overlapping availability slot.

The test should currently fail because the API returns `200 OK` instead of the expected `400 Bad Request`.

Example Postman assertions:

```javascript
pm.test("Overlapping availability should return 400", function () {
  pm.response.to.have.status(400);
});

pm.test("Correct overlap error message", function () {
  pm.expect(pm.response.text()).to.include(
    "Availability overlaps with an existing slot.",
  );
});
```

After the defect is fixed, the test should pass with:

```
Expected: 400
Actual: 400
Result: PASS
```

---

## Notes

The exact error message should follow the project's API contract.
The key acceptance criterion is that a therapist must not be able to create an availability slot that overlaps with an existing availability slot for the same therapist.
