# Test Summary

## Authentication

| Area                | Status |
| ------------------- | ------ |
| Patient Login       | Tested |
| Therapist Login     | Tested |
| Admin Login         | Tested |
| Invalid Credentials | Tested |
| Missing Fields      | Tested |
| Invalid Email       | Tested |
| Empty Body          | Tested |
| Null Values         | Tested |

## Availability

| Area                   | Status       |
| ---------------------- | ------------ |
| Create Availability    | Tested       |
| Invalid Time Range     | Tested       |
| Past Time              | Tested       |
| Overlapping Slots      | Defect Found |
| Delete Availability    | Tested       |
| Unauthorized Access    | Tested       |
| Cross-Therapist Access | Defect Found |

## Booking

| Area                | Status |
| ------------------- | ------ |
| Get Available Slots | Tested |
| Create Booking      | Tested |
| Duplicate Booking   | Tested |
| My Bookings         | Tested |

## Email Verification

| Area                   | Status       |
| ---------------------- | ------------ |
| Send Verification Code | Tested       |
| Valid Email            | Tested       |
| Missing Email          | Defect Found |
| Empty Email            | Defect Found |
| Invalid Email          | Defect Found |
| Null Email             | Tested       |
| Whitespace Email       | Defect Found |
