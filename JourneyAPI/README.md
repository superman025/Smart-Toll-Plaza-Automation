# Journey Service

The **Journey Service** is responsible for recording and managing the history of completed toll transactions by storing journey-related information.

Once a toll payment is successfully processed, the Toll Service forwards the journey data to this microservice, where it is stored for future retrieval, tracking, and reporting purposes.

This microservice acts as the dedicated journey history management component of the Smart Toll Plaza Automation System.

---

# Responsibilities

* Record completed journey details
* Maintain toll transaction history
* Retrieve stored journey records
* Search journeys using vehicle numbers
* Capture entry and exit timestamps
* Store payment status information

---

# Architecture

```text
                Client
                   │
                   ▼
          JourneyController
                   │
                   ▼
           JourneyService
                   │
                   ▼
         JourneyRepository
                   │
                   ▼
              H2 Database
```

---

# Project Structure

```text
journey-service
│
├── controller
│     JourneyController
│
├── service
│     JourneyService
│
├── repository
│     JourneyRepository
│
├── entity
│     Journey
│
├── dto
│     JourneyDTO
│
├── advice
│     ErrorResponse
│     GlobalExceptionHandler
│
├── exception
│     JourneyNotFoundException
│
└── JourneyApiApplication
```

---

# Entity

## Journey

| Field         | Type          | Description               |
| ------------- | ------------- | ------------------------- |
| id            | Integer       | Primary Identifier        |
| vehicleNumber | String        | Registered Vehicle Number |
| startTime     | LocalDateTime | Journey Start Time        |
| endTime       | LocalDateTime | Journey End Time          |
| plaza         | String        | Toll Plaza Location       |
| amount        | Double        | Toll Charge               |
| paymentStatus | String        | Payment Status            |

---

# Journey DTO

This DTO receives journey information from the Toll Service.

```text
vehicleNumber

startTime

endTime

plaza

amount

paymentStatus
```

---

# Business Flow

```text
Receive Journey Request

          │

          ▼

Validate Request

          │

          ▼

Create Journey Entity

          │

          ▼

Store in Database

          │

          ▼

Return Success Response
```

---

# REST APIs

## Save Journey

```text
POST /api/v1/journeys
```

### Request

```json
{
    "vehicleNumber":"TN38AB1234",
    "startTime":"2026-07-18T10:30:00",
    "endTime":"2026-07-18T10:35:00",
    "plaza":"Chennai Toll Plaza",
    "amount":150,
    "paymentStatus":"SUCCESS"
}
```

### Response

```text
201 CREATED
```

<img width="1917" height="1023" alt="image" src="https://github.com/user-attachments/assets/03d610db-d9da-49f6-8356-e7f6d9bf9392" />

---

## Get All Journeys

```text
GET /api/v1/journeys
```

<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/131b3bd0-288c-440d-87b9-4b540bae2a87" />

---

## Get Journey By Vehicle Number

```text
GET /api/v1/journeys/{vehicleNumber}
```

<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/6fdb3546-cacf-404d-a75f-dcb71e09fa70" />

---

# Journey Lifecycle

```text
Vehicle Crosses Toll Plaza

        │

        ▼

Payment Completed

        │

        ▼

Toll Service

        │

        ▼

Journey Service

        │

        ▼

Journey Record Saved

        │

        ▼

Journey History Updated
```

---

# Validation

All incoming requests are validated before journey data is stored.

Validation includes:

* Vehicle Number must be provided
* Toll Plaza is mandatory
* Toll Amount is required
* Payment Status is required
* Start Time is mandatory
* End Time is mandatory

---

# Payment Status

Supported values:

```text
SUCCESS

FAILED

PENDING
```

---

# Exception Handling

Global exception handling is implemented using:

```java
@RestControllerAdvice
```

### Supported Exceptions

* JourneyNotFoundException
* ValidationException

---

# Error Response

```json
{
    "statusCode":404,
    "errorType":"Journey Not Found",
    "errorMessage":"Journey not found for vehicle TN38AB1234",
    "timestamp":"2026-07-18T18:20:00"
}
```

---

# Repository

The repository extends:

```java
JpaRepository<Journey,Integer>
```

### Custom Methods

```java
findByVehicleNumber()

findAll()
```

---

# Integration

This microservice is primarily consumed by the **Toll Service** to:

* Store completed toll transactions
* Maintain journey history
* Generate travel records for future reference

---

# Logging

Application logging is implemented using **SLF4J**.

The logs capture:

* Journey creation
* Journey retrieval
* Validation errors
* Journey search requests
* Database operations

---

# Technologies

* Java 21
* Spring Boot
* Spring Web
* Spring Data JPA
* Bean Validation
* Lombok
* H2 Database
* Maven

---

# Running the Application

```bash
mvn spring-boot:run
```

### Default Port

```text
8083
```

---

# Testing

Recommended tools:

* Postman
* IntelliJ HTTP Client
* curl

---

# Future Improvements

* Journey Pagination
* Search by Date Range
* Vehicle-wise Reports
* Monthly Reports
* Excel Export
* PDF Export
* Dashboard Analytics
* Swagger/OpenAPI Documentation
* Docker Integration
* MySQL/PostgreSQL Support

---

# Author

**SRIJAI V**

B.E. Computer Science and Engineering

Saveetha Engineering College

GitHub: https://github.com/superman025/Smart-Toll-Plaza-Automation
