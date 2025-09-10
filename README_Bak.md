# Euro Denomination Calculator – Backend (Spring Boot)

## Overview

This backend service provides APIs to calculate the **Euro denomination breakdown** for a given amount.  
It supports validation, error handling, authentication, API versioning, and is integrated with Swagger for API
documentation.

---

## Features

- **Spring Boot 3 + Java 17**
- **REST APIs** with `/api/v1/...`
- **Authentication**: Basic Auth (`XXXXX/XXXX`) – credentials configurable in `application.properties`
- **Validation**: Enforces positive amount, ≤ 1,000,000, max 2 decimals
- **Exception Handling**: Centralized error handling with meaningful error responses
- **Swagger UI**: Interactive API documentation at `/swagger-ui.html`
- **Tests**:
    - Unit tests (service)
    - Controller tests (MockMvc)
    - Integration tests (with real authentication & validation)

---

## Setup & Run

### Prerequisites

- JDK 17+
- Maven 3.9+

### Run Locally

```bash
# Navigate to backend project folder
cd backend

# Build the project
mvn clean install

# Run the app
mvn spring-boot:run
```

### Run Tests

```bash
mvn test
```

---

## Authentication

Basic authentication is used.

Default credentials (defined in `application.properties`):

```properties
app.security.user.name=XXXX
app.security.user.password=XXXX
```

All API requests must include:

```
Authorization: Basic base64(username:password)
```

Example:

```bash
curl -u XXXX:XXXX -X POST http://localhost:8080/api/v1/denomination   -H "Content-Type: application/json"   -d '{"amount": 123.45}'
```

---

## API Endpoints

### 1. Calculate Denominations

**POST** `/api/v1/denomination`  
Request:

```json
{
  "amount": 123.45
}
```

Response:

```json
{
  "status": "SUCCESS",
  "currentAmount": 123.45,
  "previousAmount": 50.0,
  "result": {
    "100.0": 1,
    "20.0": 1,
    "2.0": 1,
    "1.0": 1,
    "0.2": 2,
    "0.05": 1
  },
  "difference": {
    "100.0": 1,
    "50.0": -1,
    "20.0": 1,
    "2.0": 1,
    "1.0": 1,
    "0.2": 2,
    "0.05": 1
  }
}
```

---

### 2. Get Available Denominations

**GET** `/api/v1/denomination/coins`

Response:

```json
[
  200.0,
  100.0,
  50.0,
  20.0,
  10.0,
  5.0,
  2.0,
  1.0,
  0.5,
  0.2,
  0.1,
  0.05,
  0.02,
  0.01
]
```

---

## 🏗️ Design Approach

### 1. **Architecture**

- **Controller Layer** → Exposes REST APIs (`DenominationController`)
- **Service Layer** → Business logic for denomination calculation (`EuroDenominationService`)
- **DTO Layer** → Request & Response classes (`DenominationRequest`, `DenominationResponse`)
- **Config Layer** → Security (`SecurityConfig`), OpenAPI docs (`OpenApiConfig`)
- **Exception Layer** → Centralized handling (`DenominationExceptionHandler`)

### 2. **Security**

- Basic Auth with in-memory user
- Credentials externalized to `application.properties`
- Configurable roles and users possible in future

### 3. **Validation**

- JSR-380 annotations (`@NotNull`, `@DecimalMin`, `@Max`, `@Digits`)
- Invalid input returns structured JSON error with field-level messages

### 4. **Testing**

- **Unit Tests** → Validate denomination logic (coins/notes)
- **Controller Tests (MockMvc)** → Validate API responses & validation rules
- **Integration Tests** → Run full stack (Spring context + security)

### 5. **Documentation**

- Swagger UI → `/swagger-ui.html`
- OpenAPI spec → `/v3/api-docs`

---

## 🧪 Example Tests

- **Positive**: `123.45` → correct breakdown of notes & coins
- **Negative**: `-5.0` → returns `400 Bad Request` with error
- **Negative**: Missing/Invalid credentials → returns `401 Unauthorized`

---

## 🔮 Future Improvements

1. **Authentication**

- Replace Basic Auth with JWT or OAuth2
- Add role-based access

2. **Persistence**

- Store calculation history in a database (Postgres, MySQL)

3. **Configuration**

- Allow denominations to be loaded from DB/config file

4. **Scalability**

- Dockerize and deploy with Kubernetes
- Cloud-ready microservice architecture

5. **Monitoring & Observability**

- Structured logging
- Micrometer metrics + Prometheus/Grafana dashboards

6. **Frontend Integration**

- Enrich denomination metadata (e.g., images of notes/coins)

---

**Version:** 1.0.0  
🔗 **Swagger UI:** [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)