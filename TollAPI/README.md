# Toll Service

The **Toll Service** functions as the core orchestration module of the Smart Toll Plaza Automation System.

Unlike the remaining microservices, it does **not** maintain its own database. Its primary responsibility is to coordinate with the Vehicle Service, Wallet Service, and Journey Service to complete the toll payment workflow.

When a vehicle arrives at a toll booth, this service validates the vehicle information, retrieves the associated FASTag ID, deducts the required toll amount from the wallet, records the journey details, and returns the transaction outcome.

---

# Responsibilities

* Handle toll payment requests
* Validate registered vehicle details
* Obtain FASTag information
* Process wallet balance deduction
* Store journey information
* Return payment status
* Coordinate communication among microservices

---

# Architecture

```text
                    Client
                       │
                       ▼
                TollController
                       │
                       ▼
                 TollService
                       │
      ┌────────────────┼────────────────┐
      │                │                │
      ▼                ▼                ▼
VehicleClient     WalletClient    JourneyClient
      │                │                │
      ▼                ▼                ▼
 Vehicle API      Wallet API      Journey API
```

---

# Project Structure

```text
toll-service
│
├── controller
│     TollController
│
├── service
│     TollService
│
├── client
│     VehicleClient
│     WalletClient
│     JourneyClient
│
├── dto
│     TollRequest
│     TollResponse
│     JourneyDTO
│     WalletDTO
│     VehicleDTO
│
├── config
│     RestTemplateConfig
│
└── TollApiApplication
```

---

# Why No Database?

The Toll Service is designed solely to coordinate the business workflow.

It does not maintain any permanent data.

Instead, it exchanges information with the other microservices through REST APIs.

| Service         | Responsibility               |
| --------------- | ---------------------------- |
| Vehicle Service | Validate Vehicle Information |
| Wallet Service  | Deduct Wallet Balance        |
| Journey Service | Store Journey Details        |

This implementation follows the **Database per Service** architecture, allowing each microservice to remain independent and loosely coupled.

---

# Toll Payment Workflow

```text
Receive Toll Request

        │

        ▼

Vehicle Service

Validate Vehicle

        │

        ▼

Retrieve FASTag ID

        │

        ▼

Wallet Service

Deduct Balance

        │

        ▼

Enough Balance?

        │

 ┌──────┴─────────┐

 │                │

No               Yes

 │                │

 ▼                ▼

Return Error   Journey Service

                    │

                    ▼

             Save Journey

                    │

                    ▼

          Return Success Response
```

---

# Service Communication

```text
                Toll Service

                      │

      ┌───────────────┼───────────────┐

      ▼               ▼               ▼

Vehicle API      Wallet API      Journey API

      │               │               │

Validate        Deduct Money     Save Journey

Vehicle         Update Balance   Return Success
```

---

# REST API

## Pay Toll

```text
POST /api/v1/tolls
```

### Request

```json
{
    "vehicleNumber":"TN38AB1234",
    "plaza":"Chennai Toll Plaza",
    "amount":150
}
```

### Success Response

```json
{
    "message":"Toll Paid Successfully",
    "vehicleNumber":"TN38AB1234",
    "fastagId":"FT1001",
    "amount":150,
    "status":"SUCCESS"
}
```

<img width="1917" height="958" alt="image" src="https://github.com/user-attachments/assets/eab39961-9f29-4e53-a741-39dc36baa39a" />

---

# Processing Flow

### Step 1

Accept Toll Payment Request

↓

### Step 2

Call Vehicle Service

```text
GET /vehicles/{vehicleNumber}
```

↓

Vehicle Available?

↓

No → Return Error Response

↓

Yes

↓

Fetch FASTag ID

↓

### Step 3

Call Wallet Service

```text
PUT /wallet/deduct
```

↓

Sufficient Wallet Balance?

↓

No

↓

Return Insufficient Balance Response

↓

Yes

↓

Wallet Updated Successfully

↓

### Step 4

Call Journey Service

```text
POST /journeys
```

↓

Journey Information Stored

↓

### Step 5

Return Successful Payment Response

---

# Request DTO

## TollRequest

```text
vehicleNumber

plaza

amount
```

---

# Response DTO

## TollResponse

```text
message

vehicleNumber

fastagId

amount

status
```

---

# External Clients

## VehicleClient

Responsibilities

* Validate registered vehicle details
* Retrieve the associated FASTag ID

---

## WalletClient

Responsibilities

* Deduct toll payment from the wallet
* Validate wallet information

---

## JourneyClient

Responsibilities

* Save journey records
* Return journey confirmation

---

# RestTemplate

The Toll Service communicates with the remaining microservices using:

```java
RestTemplate
```

A dedicated configuration class provides a reusable **RestTemplate Bean** for sending REST requests to external services.

---

# Error Handling

The service can handle the following failure scenarios:

* Vehicle Not Found
* FASTag Not Found
* Insufficient Wallet Balance
* Wallet Service Unavailable
* Journey Service Unavailable
* Invalid Request
* Network Timeout

---

# Transaction Lifecycle

```text
Vehicle Arrives

      │

      ▼

Vehicle Validation

      │

      ▼

Wallet Deduction

      │

      ▼

Journey Recording

      │

      ▼

Transaction Completed
```

---

# Dependencies

The Toll Service depends on the following microservices:

* Vehicle Service
* Wallet Service
* Journey Service

Ensure these services are running before starting the Toll Service.

---

# Technologies

* Java 21
* Spring Boot
* Spring Web
* RestTemplate
* Bean Validation
* Lombok
* Maven

---

# Running the Application

```bash
mvn spring-boot:run
```

### Default Port

```text
8084
```

---

# Service Startup Order

```text
Vehicle Service

        ↓

Wallet Service

        ↓

Journey Service

        ↓

Toll Service

        ↓

API Gateway
```

---

# Future Improvements

* OpenFeign Client
* Resilience4j Circuit Breaker
* Retry Mechanism
* Distributed Tracing
* Apache Kafka Integration
* Eureka Service Discovery
* Docker Support
* Kubernetes Deployment
* JWT-Based Authentication
* Swagger/OpenAPI Documentation

---

# Author

**SRIJAI V**

B.E. Computer Science and Engineering

Saveetha Engineering College

GitHub: https://github.com/superman025/Smart-Toll-Plaza-Automation
