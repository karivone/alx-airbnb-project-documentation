
# Technical and Functional Requirements

## 1. User Authentication

### Functional Requirements:
- Users can register, log in, and log out securely.
- Passwords must be hashed before storage.
- Duplicate emails must be rejected.

### API Endpoints:
- **POST /api/register** 
  Input: JSON `{ "first_name", "last_name", "email", "password" }` 
  Output: 201 Created, User object 
  Validations:
   - Email must be unique and valid format.
   - Password must be at least 8 characters.

- **POST /api/login** 
  Input: JSON `{ "email", "password" }` 
  Output: 200 OK, JWT token 
  Validations:
   - Email must exist in the database.
   - Password must match hash.

### Performance Criteria:
- Auth responses < 200ms under load.
- JWT expiration set to 24 hours.

---

## 2. Property Management

### Functional Requirements:
- Hosts can create, update, and delete properties.
- Guests can view and search properties.

### API Endpoints:
- **POST /api/properties** 
  Input: JSON `{ "host_id", "name", "description", "location", "pricepernight" }` 
  Output: 201 Created, Property object 
  Validations:
   - `pricepernight` must be a positive decimal.
   - Host must be a valid user with role "host".

- **GET /api/properties?location=city** 
  Output: 200 OK, List of matching Property objects

### Performance Criteria:
- Full property search returns results in < 500ms.
- Support indexing on location and price.

---

## 3. Booking System

### Functional Requirements:
- Guests can book available properties.
- Prevent overlapping bookings.
- Calculate total price automatically.

### API Endpoints:
- **POST /api/bookings** 
  Input: JSON `{ "property_id", "user_id", "start_date", "end_date" }` 
  Output: 201 Created, Booking object with `total_price` 
  Validations:
   - Dates must be valid and not in the past.
   - Check if the property is available in that date range.
   - Calculate `total_price = pricepernight * number_of_nights`

### Performance Criteria:
- Booking checks and creation complete in < 300ms.
- Overlapping date validation must be atomic.

