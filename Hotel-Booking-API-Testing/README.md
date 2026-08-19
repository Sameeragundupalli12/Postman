# Hotel Booking API Testing

**API testing project using Postman and Newman for the Restful-Booker public REST API.**

This project demonstrates functional API testing, positive and negative testing, API chaining, environment variables, response validation, JavaScript-based assertions, and automated test execution using Newman.

## Project Overview

The project covers the main booking workflows of the Restful-Booker API, including:

* Health check
* Retrieve all bookings
* Create authentication token
* Create booking
* Retrieve booking by ID
* Retrieve created booking
* Update booking
* Negative testing for invalid authentication
* Negative testing for missing required fields
* Negative testing for invalid booking ID
* Filter bookings by first name
* Filter bookings by last name
* Delete booking
* Verify deleted booking cannot be retrieved

## Test Execution Summary

| Metric                | Result |
| --------------------- | -----: |
| API Requests Executed |     15 |
| API Requests Failed   |      0 |
| Assertions            |     46 |
| Assertions Failed     |      0 |

**Final Result: 15/15 API requests executed successfully, with 46/46 assertions passed and 0 failures.**

## Tools & Technologies

* **Postman** – API functional testing and test automation
* **Newman** – Command-line execution of Postman collections
* **JavaScript** – Postman test scripts and assertions
* **REST API** – API testing
* **JSON** – Request and response data validation
* **Git & GitHub** – Version control and project sharing

## API Tested

**Restful-Booker API**

**Base URL:** https://restful-booker.herokuapp.com

**API Documentation:** https://restful-booker.herokuapp.com/apidoc/index.html

## Test Scenarios

|  # | Test Scenario                       | Expected Status |
| -: | ----------------------------------- | --------------: |
|  1 | Health Check                        |             201 |
|  2 | Get All Bookings                    |             200 |
|  3 | Create Auth Token                   |             200 |
|  4 | Create Booking                      |             200 |
|  5 | Get Booking By ID                   |             200 |
|  6 | Get Created Booking                 |             200 |
|  7 | Update Booking                      |             200 |
|  8 | Update Booking - Invalid Token      |             403 |
|  9 | Update Booking - Missing Field      |             400 |
| 10 | Get Booking - Invalid ID            |             404 |
| 11 | Create Booking - Missing Field      |             500 |
| 12 | Get Bookings - Filter by First Name |             200 |
| 13 | Get Bookings - Filter by Last Name  |             200 |
| 14 | Delete Booking                      |             201 |
| 15 | Get Deleted Booking                 |             404 |

> **Note:** Expected status codes reflect the observed behavior of the Restful-Booker API during test execution. For example, the **Create Booking - Missing Field** scenario returns HTTP 500 from the API itself, and the test validates that this is the response actually received.

## Postman Concepts Demonstrated

### HTTP Methods

* **GET** – Retrieve booking information
* **POST** – Create authentication token and bookings
* **PUT** – Update booking information
* **DELETE** – Delete a booking

### Request & Response Validation

The project includes validation for:

* HTTP status codes
* JSON response format
* Response structure
* Required fields
* Data types
* Response body content
* Booking data
* Request and response data comparison

## Environment Variables

The project uses Postman environment variables for dynamic values:

* `base_url`
* `token`
* `booking_id`

The authentication token and booking ID are extracted from API responses and stored dynamically using `pm.environment.set()`. These values are then reused in subsequent requests.

This demonstrates dynamic API chaining without manually entering values between requests.

### Environment File

The Postman environment JSON file is **intentionally not included in this public GitHub repository**.

The collection can be executed by creating a local Postman environment containing:

```text
base_url = https://restful-booker.herokuapp.com
```

The `token` and `booking_id` values are generated dynamically during execution and do not need to be entered manually.

For Newman execution, `base_url` can also be supplied directly through the command line using `--env-var`.

## API Chaining

The collection demonstrates API chaining between requests:

```text
Create Auth Token
        ↓
Store token
        ↓
Create Booking
        ↓
Store booking_id
        ↓
Get Booking By ID
        ↓
Update Booking
        ↓
Delete Booking
        ↓
Verify Deleted Booking
```

The `token` and `booking_id` values are dynamically extracted from API responses and stored as environment variables for use in subsequent requests.

## Postman Test Scripts

JavaScript-based Postman test scripts are used to validate API responses.

### Status Code Validation

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

### Response Structure and Data Type Validation

```javascript
pm.test("bookingid exists and is a number", function () {
    var jsonData = pm.response.json();

    pm.expect(jsonData).to.have.property("bookingid");
    pm.expect(jsonData.bookingid).to.be.a("number");
});
```

Additional assertions are used to validate response fields, data types, response content, and booking information.

## Newman Execution

Newman was used to execute the Postman collection from the command line.

Since the Postman environment file is not included in this repository, `base_url` is supplied directly using `--env-var`.

### Basic Newman Command

```bash
newman run "hotel-booking-api-collection.json" \
    --env-var "base_url=https://restful-booker.herokuapp.com"
```

### Newman HTML Report

An HTML execution report was generated using the **Newman HTML Extra Reporter**.

```bash
newman run "hotel-booking-api-collection.json" \
    --env-var "base_url=https://restful-booker.herokuapp.com" \
    -r cli,htmlextra \
    --reporter-htmlextra-export "newman-report.html"
```

The generated `newman-report.html` file contains detailed test execution results and is included in this repository.

## Project Structure

```text
Hotel-Booking-API-Testing/
│
├── hotel-booking-api-collection.json
├── newman-report.html
└── README.md
```

## Notes

* This project uses the public Restful-Booker sandbox API for testing purposes only; no real customer or booking information is involved.
* The health check provides a quick smoke validation that the API is reachable before functional tests run.
