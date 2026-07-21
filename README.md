<div align="center">

# Smart Toll Plaza Automation System

### A Spring Boot Microservices-Driven Toll Collection Platform

An intelligent toll collection system developed using **FASTag**, **REST APIs**, and **Spring Boot Microservices** to simplify highway toll operations.

![Java](https://img.shields.io/badge/Java-21-1565C0?style=for-the-badge&logo=openjdk&logoColor=FFFFFF)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-2E7D32?style=for-the-badge&logo=springboot&logoColor=FFFFFF)
![API Gateway](https://img.shields.io/badge/API_Gateway-6A1B9A?style=for-the-badge&logo=cloudflare&logoColor=FFFFFF)
![H2 Database](https://img.shields.io/badge/H2_Database-00838F?style=for-the-badge&logo=sqlite&logoColor=FFFFFF)
![REST API](https://img.shields.io/badge/REST_API-E65100?style=for-the-badge&logo=postman&logoColor=FFFFFF)
![Microservices](https://img.shields.io/badge/Microservices-0D47A1?style=for-the-badge&logo=docker&logoColor=FFFFFF)

</div>

---

# Overview

The **Smart Toll Plaza Automation System** is a capstone project built with Spring Boot Microservices to modernize toll collection through FASTag-enabled digital payments.

This solution eliminates conventional manual toll collection by validating registered vehicles, checking FASTag wallet balances, deducting toll charges, storing travel information, and routing every client request through a centralized API Gateway.

Each module functions as an independent Spring Boot microservice following a layered architecture and interacts with other services through synchronous REST APIs.

---

# Objectives

* Automate toll payment transactions
* Manage registered vehicle information
* Perform FASTag wallet management
* Store and monitor journey records
* Enable seamless microservice communication
* Route all client requests through the API Gateway

---

# System Architecture

```text
                    Client
                       │
                       ▼
               API Gateway (8080)
                       │
      ┌────────────────┼────────────────┐
      │                │                │
      ▼                ▼                ▼
 Vehicle Service   Wallet Service   Journey Service
      │
      │
      ▼
   Toll Service
```

---

# Microservices

| Service         | Description                                   |                                                                  
| --------------- | --------------------------------------------------------- |
| Vehicle Service | Manages vehicle registration and FASTag information       | 
| Wallet Service  | Handles FASTag wallet recharge and payment operations     |
| Journey Service | Stores and retrieves vehicle journey records              | 
| Toll Service    | Coordinates the overall toll payment workflow             | 
| API Gateway     | Serves as the central entry point for all client requests | 


# Toll Payment Flow

```text
Vehicle Number

      │
      ▼

Vehicle Service

      │
      ▼

Retrieve FASTag ID

      │
      ▼

Wallet Service

      │
      ▼

Deduct Available Balance

      │
      ▼

Journey Service

      │
      ▼

Store Journey Details

      │
      ▼

Return Payment Success Response
```

---

# Repository Structure

```text
smart-toll-plaza-automation
│
├── api-gateway
│
├── vehicle-service
│
├── wallet-service
│
├── journey-service
│
├── toll-service
│
└── README.md
```

---

# Features

## Vehicle Service

* Register New Vehicles
* Update Vehicle Information
* Delete Vehicle Records
* Retrieve Vehicle Details
* Generate Unique Vehicle Numbers
* Generate FASTag IDs

---

## Wallet Service

* Create Wallet
* Recharge Wallet Balance
* Deduct Toll Charges
* Check Wallet Balance
* Prevent Negative Balance Transactions

---

## Journey Service

* Save Journey Records
* View Travel History
* Search Using Vehicle Number

---

## Toll Service

* Validate Registered Vehicles
* Retrieve FASTag Information
* Perform Wallet Deduction
* Create Journey Logs
* Return Payment Status

---

## API Gateway

* Route Incoming Requests
* Single Entry Point for APIs
* Simplified Client-Service Interaction

---

# Tech Stack

## Backend

* Java 21
* Spring Boot
* Spring Web
* Spring Data JPA
* Spring Validation
* Spring Cloud Gateway

---

## Database

* H2 Database

---

## Communication

* REST APIs
* RestTemplate

---

## Development Tools

* IntelliJ IDEA
* Postman
* Git
* GitHub
* Maven

---

# Project Structure

Every microservice follows a clean layered architecture.

```text
service-name
│
├── controller
├── service
├── repository
├── entity
├── dto
├── advice
├── exception
└── config
```

---

# Running the Project

Start the microservices in the following order.

```
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

# Default Ports

| Service         | Port |
| --------------- | ---- |
| API Gateway     | 8080 |
| Vehicle Service | 8081 |
| Wallet Service  | 8082 |
| Journey Service | 8083 |
| Toll Service    | 8084 |

---

# API Gateway Routes

| Route        | Destination     |
| ------------ | --------------- |
| /vehicles/** | Vehicle Service |
| /wallet/**   | Wallet Service  |
| /journeys/** | Journey Service |
| /toll/**     | Toll Service    |

---

# Complete Workflow

1. Register the Vehicle
2. Create a FASTag Wallet
3. Add Funds to the Wallet
4. Initiate Toll Payment
5. Deduct the Required Amount
6. Store Journey Information
7. Return Payment Confirmation

---

# Core Concepts Used

* Spring Boot
* RESTful APIs
* Microservices Architecture
* Layered Design Pattern
* DTO Pattern
* Bean Validation
* Global Exception Handling
* RestTemplate
* API Gateway
* H2 Database
* Logging
* CRUD Operations

---

# Future Scope

* Eureka Service Discovery
* OpenFeign Integration
* MySQL / PostgreSQL Compatibility
* Docker Containerization
* JWT Authentication
* Spring Security
* Apache Kafka Messaging
* Redis Caching
* Swagger / OpenAPI Support
* Kubernetes Deployment

---

# Author

**SRIJAI V**

B.E. Computer Science and Engineering

Saveetha Engineering College

GitHub:

https://github.com/superman025/Smart-Toll-Plaza-Automation

---
