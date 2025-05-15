# Backend Requirement Specifications – Airbnb Clone

This document outlines the functional and technical requirements for core backend features of the Airbnb Clone project. The specifications cover API endpoints, expected input/output, validation rules, and performance criteria.


## 1. User Authentication

### 1.1. Register User

- **Endpoint:** `POST /api/auth/register/`
- **Description:** Registers a new user as a guest or host.

#### Input:
```json
{
  "email": "user@example.com",
  "password": "securePassword123",
  "role": "host", 
  "full_name": "John Doe"
}
```
#### Output:
```json
{
  "message": "User registered successfully",
  "token": "<jwt_token>"
}
```
### Validation Rules:
- Email must be unique and valid.

- Password must be at least 8 characters.

- Role must be host or guest.

### Performance:
- Average response time < 500ms.

### 1.2. Login User
- **Endpoint:** `POST /api/auth/login/`

- **Description:** Authenticates an existing user and returns a JWT token.

#### Input:
```json
{
  "email": "user@example.com",
  "password": "securePassword123"
}
``` 
#### Output:
```json
{
  "token": "<jwt_token>",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "role": "host"
  }
}
```
### Validation Rules:
- Valid email and password required.

## 2. Property Management

### 2.1 Add New Listing

- **Endpoint:** `POST /api/properties/`

- **Description:** Allows a host to add a property listing.

- **Input:**

```json
{
  "title": "Beachfront Apartment",
  "description": "Beautiful 2-bedroom apartment near the sea.",
  "location": "Miami, FL",
  "price": 150.00,
  "amenities": ["WiFi", "Air Conditioning"],
  "availability": {
    "start_date": "2025-06-01",
    "end_date": "2025-08-31"
  }
}
```
### Output:
```json
{
  "message": "Listing created successfully",
  "property_id": 101
}
```
### Validation Rules:

- Title and description are required.

- Price must be a positive float.

- Dates must be valid and non-overlapping.

### 2.2 Edit/Delete Listing
- Endpoints:

 - **Edit**: `PUT /api/properties/{id}/`

- **Delete**: `DELETE /api/properties/{id}/`

- **Authentication**: Host-only action.

### Validation Rules:

- Host must own the property.

- Cannot delete a property with active bookings.

## 3. Booking System
### 3.1 Create Booking
- **Endpoint**: `POST /api/bookings/`

- **Description**: Allows guests to book available properties.

- **Input**:
```json
{
  "property_id": 101,
  "check_in": "2025-07-01",
  "check_out": "2025-07-05"
}
```
- **Output**:
```json
{
  "message": "Booking successful",
  "booking_id": 202
}
```
#### Validation Rules:

- Check-in and check-out dates must be valid.

- Property must be available.

- Prevent double booking.

### 3.2 Cancel Booking
- **Endpoint**: `DELETE /api/bookings/{id}/`

- **Access**: Guest who booked or host of the property.

- **Output**:
```json
{
  "message": "Booking cancelled successfully"
}
```
#### Rules:

- Cancellations must comply with cancellation policy.

- Cannot cancel completed or expired bookings.

### General Performance Criteria
- All endpoints should return responses within 300–700ms under normal load.

- Error handling must return consistent error structures.

- JWT authentication is required for all protected endpoints.