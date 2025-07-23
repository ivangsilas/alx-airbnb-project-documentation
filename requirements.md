# Airbnb Project – Backend Requirements

This document outlines the technical and functional requirements for the key backend features of the Airbnb-like system.

---

## 1. User Authentication

### 📌 Functionality
- Register new users
- Authenticate existing users
- Maintain session/token-based login

### 📎 API Endpoints

| Method | Endpoint           | Description             |
|--------|--------------------|-------------------------|
| POST   | /api/v1/auth/register | Register new user       |
| POST   | /api/v1/auth/login    | Login user and get token|
| GET    | /api/v1/auth/profile  | Get current user data   |

### 📥 Input Specifications

- `email`: string, required, must be a valid email
- `password`: string, required, min 8 characters
- `name`: string, required (register only)

### 📤 Output (Example)
```json
{
  "token": "JWT_TOKEN",
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "name": "Jane Doe"
  }
}
```

### ✅ Validation Rules
- Email must be unique
- Password must be encrypted (e.g., bcrypt)

### ⚙️ Performance Criteria
- Login response time: < 500ms
- 99.9% uptime for authentication endpoints

---

## 2. Property Management

### 📌 Functionality
- Hosts can create, view, update, and delete their properties

### 📎 API Endpoints

| Method | Endpoint             | Description                 |
|--------|----------------------|-----------------------------|
| POST   | /api/v1/properties   | Create new property         |
| GET    | /api/v1/properties   | List all available properties|
| GET    | /api/v1/properties/:id | Get property details      |
| PUT    | /api/v1/properties/:id | Update property           |
| DELETE | /api/v1/properties/:id | Delete property           |

### 📥 Input Specifications

- `title`: string, required
- `description`: string, optional
- `price`: float, required
- `location`: string, required
- `available`: boolean, default `true`

### 📤 Output (Example)
```json
{
  "id": "property_uuid",
  "title": "2-Bedroom Apartment",
  "price": 120.00,
  "available": true
}
```

### ✅ Validation Rules
- Price must be a positive number
- Host must be authenticated
- Cannot delete a property with active bookings

### ⚙️ Performance Criteria
- Property creation: < 700ms
- Search/filter query: < 300ms for < 10,000 properties

---

## 3. Booking System

### 📌 Functionality
- Guests can book available properties
- System checks availability and prevents double bookings

### 📎 API Endpoints

| Method | Endpoint            | Description         |
|--------|---------------------|---------------------|
| POST   | /api/v1/bookings    | Create a booking    |
| GET    | /api/v1/bookings    | View guest bookings |
| DELETE | /api/v1/bookings/:id | Cancel a booking   |

### 📥 Input Specifications

- `property_id`: UUID, required
- `start_date`: date, required
- `end_date`: date, required

### 📤 Output (Example)
```json
{
  "booking_id": "uuid",
  "property_id": "uuid",
  "user_id": "uuid",
  "start_date": "2025-08-01",
  "end_date": "2025-08-05",
  "status": "confirmed"
}
```

### ✅ Validation Rules
- End date must be after start date
- Booking must not overlap with existing bookings
- Only authenticated users can book

### ⚙️ Performance Criteria
- Availability check latency: < 500ms
- Booking creation: < 800ms

---

## 📌 Notes

- All endpoints require JWT token authentication except register/login.
- API responses must follow RESTful structure and return HTTP codes accordingly.