# Vehicle Service

The **Vehicle Service** manages vehicle registration details and FASTag information within the Smart Toll Plaza Automation System.

It serves as the primary source of vehicle data for the entire application. Before processing any toll payment, the Toll Service communicates with this service to verify the registered vehicle and obtain its corresponding FASTag ID.

---

# Responsibilities

* Register vehicles into the system
* Maintain FASTag information
* Fetch vehicle details
* Update existing vehicle records
* Delete vehicle information
* Guarantee unique vehicle registration numbers
* Ensure every FASTag ID is unique

---

# Architecture

```text
                Client
                   │
                   ▼
          VehicleController
                   │
                   ▼
            VehicleService
                   │
                   ▼
         VehicleRepository
                   │
                   ▼
              H2 Database
```

---

# Project Structure

```text
vehicle-service
│
├── controller
│     VehicleController
│
├── service
│     VehicleService
│
├── repository
│     VehicleRepository
│
├── entity
│     Vehicle
│
├── dto
│     VehicleDTO
│
├── enums
│     VehicleType
│
├── advice
│     ErrorResponse
│     GlobalExceptionHandler
│
├── exception
│     DuplicateVehicleException
│     FastTagNotFoundException
│     VehicleNotFoundException
│
└── VehicleApiApplication
```

---

# Entity

## Vehicle

| Field         | Type    | Description                        |
| ------------- | ------- | ---------------------------------- |
| id            | Integer | Primary Identifier                 |
| vehicleNumber | String  | Unique Vehicle Registration Number |
| ownerName     | String  | Registered Owner's Name            |
| vehicleType   | Enum    | CAR / BUS / TRUCK / BIKE           |
| fastagId      | String  | Unique FASTag Number               |

---

# DTO

## VehicleDTO

This DTO is used to receive vehicle information from the client application.

```java
vehicleNumber

ownerName

vehicleType

fastagId
```

Validation performed includes:

* Vehicle registration number format verification
* Mandatory field validation
* Owner name length validation
* Vehicle type validation
* FASTag ID presence validation

---

# Vehicle Types

The application supports the following vehicle categories:

```text
CAR

BUS

TRUCK

BIKE
```

---

# Business Flow

```text
Register Vehicle

       │

       ▼

Validate Request

       │

       ▼

Vehicle Number Exists?

       │

 ┌─────┴──────┐

 │            │

Yes          No

 │            │

 ▼            ▼

Throw      FASTag Exists?

Exception       │

         ┌─────┴─────┐

         │           │

       Yes          No

         │           │

         ▼           ▼

      Throw      Save Vehicle

      Exception      │

                     ▼

             Return Created
```

---

# REST APIs

## Register Vehicle

```text
POST /api/v1/vehicles
```

### Request

```json
{
  "vehicleNumber":"TN38AB1234",
  "ownerName":"Srijai",
  "vehicleType":"CAR",
  "fastagId":"FT1001"
}
```

### Response

```text
201 CREATED
```

#### Example Output

<img width="1917" height="915" alt="image" src="https://github.com/user-attachments/assets/cdcd331a-b6fa-4b0c-9a5e-8969feefe29d" />

---

## Get Vehicle

```text
GET /api/v1/vehicles/{vehicleNumber}
```

<img width="1917" height="873" alt="image" src="https://github.com/user-attachments/assets/2823893b-155f-4038-813e-efedc24fba0c" />

---

## Get All Vehicles

```text
GET /api/v1/vehicles
```

<img width="1912" height="1017" alt="image" src="https://github.com/user-attachments/assets/69ace1c4-af9e-4d5d-95dc-def13c92fac5" />

---

## Update Vehicle

```text
PUT /api/v1/vehicles/{id}
```

<img width="1917" height="925" alt="image" src="https://github.com/user-attachments/assets/3b18404b-942e-49ef-9501-367826131283" />

---

## Delete Vehicle

```text
DELETE /api/v1/vehicles/{id}
```

<img width="1917" height="602" alt="image" src="https://github.com/user-attachments/assets/afef7c41-4539-48a3-9381-57a135e3cf14" />

---

# Validation

| Field          | Validation                     |
| -------------- | ------------------------------ |
| Vehicle Number | Required + Pattern Validation  |
| Owner Name     | Minimum Length of 3 Characters |
| Vehicle Type   | Enum Validation                |
| FASTag ID      | Required Field                 |

Example:

```text
TN42IK2007
```

---

# Exception Handling

Global exception handling is implemented using:

```java
@RestControllerAdvice
```

### Supported Exceptions

* DuplicateVehicleException
* VehicleNotFoundException
* FastTagNotFoundException
* MethodArgumentNotValidException

---

# Error Response

A standardized error response is returned whenever an exception occurs.

```json
{
  "statusCode":400,
  "errorType":"Validation Failed",
  "errorMessage":"Vehicle already exists",
  "timestamp":"2026-07-18T12:00:00"
}
```

---

# Repository

The repository interface extends:

```java
JpaRepository<Vehicle,Integer>
```

### Custom Repository Methods

```java
existsByVehicleNumber()

existsByFastagId()

findByVehicleNumber()
```

---

# Logging

Application logging is implemented using **SLF4J**.

The logs capture:

* Vehicle registration requests
* Vehicle lookup operations
* Vehicle update activities
* Vehicle deletion events
* Duplicate vehicle detection
* Missing vehicle records

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
8081
```

---

# Testing

The following tools can be used to test the APIs:

* Postman
* IntelliJ HTTP Client
* curl

---

# Integration

The **Toll Service** consumes this microservice to:

* Verify registered vehicle information
* Retrieve the linked FASTag ID

---

# Future Improvements

* Pagination Support
* Advanced Search Functionality
* Swagger/OpenAPI Integration
* MySQL Database Support
* Unit & Integration Testing
* Docker Containerization
* OpenFeign Communication
* Eureka Service Discovery

---

# Author

**SRIJAI V**

B.E. Computer Science and Engineering

Saveetha Engineering College

GitHub: https://github.com/superman025/Smart-Toll-Plaza-Automation
