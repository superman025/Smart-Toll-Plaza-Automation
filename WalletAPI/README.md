# Wallet Service

The **Wallet Service** manages all FASTag wallet-related activities within the Smart Toll Plaza Automation System.

It is responsible for creating wallets, maintaining available balances, processing wallet recharges, deducting toll fees, and validating whether sufficient funds are available before completing a toll transaction.

Whenever a toll payment is initiated, the Toll Service communicates with this microservice to perform the wallet operations.

---

# Responsibilities

* Register FASTag wallets
* Add funds to the wallet
* Deduct toll payment amounts
* Retrieve wallet details
* Remove wallet records
* Maintain wallet balances
* Ensure every FASTag ID is unique
* Validate available balance before deduction

---

# Architecture

```text
                Client
                   │
                   ▼
           WalletController
                   │
                   ▼
            WalletService
                   │
                   ▼
          WalletRepository
                   │
                   ▼
              H2 Database
```

---

# Project Structure

```text
wallet-service
│
├── controller
│     WalletController
│
├── service
│     WalletService
│
├── repository
│     WalletRepository
│
├── entity
│     Wallet
│
├── dto
│     WalletRequest
│     RechargeRequest
│     DeductRequest
│
├── advice
│     ErrorResponse
│     GlobalExceptionHandler
│
├── exception
│     DuplicateFastTagException
│     FastTagNotFoundException
│     InsufficientBalanceException
│     InvalidAmountException
│
└── WalletApiApplication
```

---

# Entity

## Wallet

| Field    | Type    | Description            |
| -------- | ------- | ---------------------- |
| id       | Integer | Primary Identifier     |
| fastagId | String  | Unique FASTag Number   |
| balance  | Double  | Current Wallet Balance |

---

# DTOs

## WalletRequest

Used to create a new wallet.

```text
fastagId
```

---

## RechargeRequest

```text
fastagId

amount
```

---

## DeductRequest

```text
fastagId

amount
```

---

# Business Flow

## Create Wallet

```text
Client

   │

   ▼

FASTag Exists?

   │

Yes──────────────► Throw Exception

   │

No

   │

Create Wallet

   │

Balance = 0

   │

Return Created
```

---

## Recharge Wallet

```text
Receive Request

      │

      ▼

FASTag Exists?

      │

No────────► Exception

      │

Yes

      ▼

Amount > 100 ?

      │

No────────► Invalid Amount

      │

Yes

      ▼

Update Balance

      │

Save Wallet

      │

Return Success
```

---

## Deduct Wallet Balance

```text
Receive Request

      │

      ▼

FASTag Exists?

      │

No────────► Exception

      │

Yes

      ▼

Sufficient Balance?

      │

No────────► Insufficient Balance

      │

Yes

      ▼

Deduct Amount

      │

Save Wallet

      ▼

Return Updated Wallet
```

---

# REST APIs

## Create Wallet

```text
POST /api/v1/wallets
```

### Request

```json
{
    "fastagId":"FT1001"
}
```

### Response

```text
201 CREATED
```

<img width="1917" height="841" alt="image" src="https://github.com/user-attachments/assets/afd93c08-54ac-470c-aa88-65fc67988f0e" />

---

## Get Wallet

```text
GET /api/v1/wallets/{fastagId}
```

<img width="1917" height="572" alt="image" src="https://github.com/user-attachments/assets/3817bdcd-1969-46a8-a4cf-8dfce9fe1583" />

---

## Get All Wallets

```text
GET /api/v1/wallets
```

<img width="1882" height="775" alt="image" src="https://github.com/user-attachments/assets/7f5ecd26-28d1-46a8-bb77-e1d83f44fef1" />

---

## Recharge Wallet

```text
PUT /api/v1/wallets/recharge
```

### Request

```json
{
    "fastagId":"FT1001",
    "amount":500
}
```

<img width="1917" height="852" alt="image" src="https://github.com/user-attachments/assets/275cc85c-93eb-44ec-a55c-d9adaa408279" />

---

## Deduct Wallet

```text
PUT /api/v1/wallets/deduct
```

### Request

```json
{
    "fastagId":"FT1001",
    "amount":150
}
```

<img width="1917" height="841" alt="image" src="https://github.com/user-attachments/assets/5ed1a892-7ce0-4646-a909-5102fa8a7a2f" />

---

## Delete Wallet

```text
DELETE /api/v1/wallets/{id}
```

<img width="1917" height="487" alt="image" src="https://github.com/user-attachments/assets/bd42d6c9-e03d-4525-a420-1d5c52b147d5" />

---

# Wallet Rules

## Wallet Creation

* Each FASTag ID must be registered only once.
* Every new wallet starts with an initial balance of **₹0**.

---

## Recharge Rules

* Recharge is allowed only for existing FASTag IDs.
* The recharge amount should be greater than **₹100**.

---

## Deduction Rules

* The FASTag ID must be valid.
* The wallet must contain sufficient funds.
* The available balance is updated immediately after a successful transaction.

---

# Exception Handling

Global exception handling is implemented using:

```java
@RestControllerAdvice
```

### Supported Exceptions

* DuplicateFastTagException
* FastTagNotFoundException
* InvalidAmountException
* InsufficientBalanceException
* ValidationException

---

# Error Response

```json
{
    "statusCode":400,
    "errorType":"Validation Failed",
    "errorMessage":"Amount must be greater than 100",
    "timestamp":"2026-07-18T15:20:00"
}
```

---

# Repository

The repository extends:

```java
JpaRepository<Wallet,Integer>
```

### Custom Repository Methods

```java
findByFastagId()

existsByFastagId()
```

---

# Logging

The application uses **SLF4J** to maintain logs.

The logging module records:

* Wallet creation
* Recharge transactions
* Balance deductions
* Duplicate FASTag detection
* Invalid recharge requests
* Insufficient balance events
* Wallet deletion operations

---

# Integration

This microservice is utilized by the **Toll Service** to:

* Process toll payment deductions
* Verify wallet balance before completing the payment

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
8082
```

---

# Testing

Recommended API testing tools:

* Postman
* IntelliJ HTTP Client
* curl

---

# Future Improvements

* Transaction History
* Recharge History
* Wallet Statements
* Daily Transaction Limits
* Automatic Wallet Recharge
* JWT-Based Authentication
* Swagger/OpenAPI Documentation
* Docker Integration
* MySQL/PostgreSQL Support
* OpenFeign Client

---

# Author

**SRIJAI V**

B.E. Computer Science and Engineering

Saveetha Engineering College

GitHub: https://github.com/superman025/Smart-Toll-Plaza-Automation
