# Denomination Calculator (Backend + Frontend)

This project provides a **Euro Denomination Calculator** with a **Spring Boot backend** and an **Angular frontend**.

## 🔹 Backend
- Spring Boot 3 (Java 17, Maven)
- Endpoints:
  - `POST /api/v1/denomination`
  - `POST /api/v1/denomination/full`
- Lombok to remove boilerplate
- Swagger UI: `/swagger-ui.html`
- JUnit + MockMvc + Integration Tests
- Basic Auth: `XXXX` / `XXXX`

## 🔹 Frontend
- Angular 18 (no Angular Material)
- Shows denominations List of Notes and Coins used (`Note`/`Coin`)
- Displays "New Amount" and "Difference to Old Amount"
- Basic Auth configured in environment

## 🔹 Running
- Start backend with `mvn spring-boot:run`
- Start frontend with `npm start`
- Open frontend at `http://localhost:4200`

## 🔹 Structure
```
denomination-app/
 ├── backend/   # Spring Boot API (full source code)
 └── frontend/  # Angular UI (full source code)
```
