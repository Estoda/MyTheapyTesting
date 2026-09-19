# BUG-002: Therapist Can Delete Another Therapist's Availability

## Summary

A therapist is able to delete an availability slot that belongs to another therapist.

---

## Environment

- **API**: `http://mytherapy.runasp.net`
- **Endpoint**: `DELETE /api/therapist/availability/{id}`
- **Role used**: Therapist
- **Authentication**: Bearer token

---

## Preconditions

1. Two therapist accounts exist: Therapist A and Therapist B.
2. Therapist B has an availability slot.
3. Therapist A is authenticated with a valid therapist token.
4. Therapist A knows the ID of Therapist B's slot.

---

## Steps to Reproduce

1. Log in as Therapist B and create an availability slot.
2. Store the returned `slotId`.
3. Log in/use the token for Therapist A.
4. Send:
   `DELETE /api/therapist/availability/{therapistBSlotId}`
5. Observe the response.

---

## Expected Result

The API should prevent Therapist A from deleting Therapist B's availability.

Expected HTTP status:

- `403 Forbidden` if the API explicitly rejects the unauthorized operation, or
- `404 Not Found` if the API intentionally hides resources owned by another therapist.

The important requirement is that the slot must **not be deleted** by Therapist A.

---

## Actual Result

- HTTP status: `200 OK`
- Response body: empty
- Therapist A successfully deletes Therapist B's availability slot.

---

## Impact

This is an authorization/access-control issue. A therapist can modify/delete another therapist's availability, which can affect scheduling and the availability data of other therapists.

---

## Suggested Severity

**High** — pending confirmation of the application's business impact and production exposure.

---

## Regression Test

Keep an automated test that verifies a therapist cannot delete another therapist's slot.

The test should fail with the current implementation because the API returns `200`.

---

## Notes

The exact expected error status should follow the project's API contract. The key acceptance criterion is that a therapist must not be able to delete another therapist's availability.
