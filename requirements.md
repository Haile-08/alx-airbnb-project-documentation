# Airbnb Clone Backend Requirement Specifications

## Feature 1: User Authentication

**Objective:** Allow users to securely register, log in, and manage sessions.

### API Endpoints

1. **POST** `/api/auth/register`

   - **Input:**
     ```json
     {
       "name": "John Doe",
       "email": "johndoe@example.com",
       "password": "securepassword123",
       "role": "guest" // or "host"
     }
     ```
   - **Output (Success):**
     ```json
     {
       "message": "Registration successful",
       "userId": "12345"
     }
     ```
   - **Output (Error):**
     ```json
     {
       "error": "Email already exists"
     }
     ```
   - **Validation Rules:**
     - Name: Required, string, max 50 characters.
     - Email: Required, valid email format.
     - Password: Required, minimum 8 characters, must include at least one uppercase letter, one number, and one special character.

2. **POST** `/api/auth/login`

   - **Input:**
     ```json
     {
       "email": "johndoe@example.com",
       "password": "securepassword123"
     }
     ```
   - **Output (Success):**
     ```json
     {
       "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
       "user": {
         "id": "12345",
         "name": "John Doe",
         "role": "guest"
       }
     }
     ```
   - **Output (Error):**
     ```json
     {
       "error": "Invalid credentials"
     }
     ```

3. **GET** `/api/auth/profile`
   - **Headers:**
     ```json
     {
       "Authorization": "Bearer <JWT-Token>"
     }
     ```
   - **Output:**
     ```json
     {
       "id": "12345",
       "name": "John Doe",
       "email": "johndoe@example.com",
       "role": "guest"
     }
     ```

### Performance Criteria

- Token generation: <200ms per request.
- Endpoint response: <500ms with database validation.

---

## Feature 2: Property Management

**Objective:** Allow hosts to manage property listings.

### API Endpoints

1. **POST** `/api/properties`

   - **Input:**
     ```json
     {
       "title": "Cozy Apartment",
       "description": "A beautiful place near the city center",
       "location": "New York, NY",
       "price": 120,
       "amenities": ["Wi-Fi", "Air Conditioning"],
       "availability": {
         "startDate": "2024-01-01",
         "endDate": "2024-12-31"
       }
     }
     ```
   - **Output (Success):**
     ```json
     {
       "message": "Property listed successfully",
       "propertyId": "98765"
     }
     ```
   - **Validation Rules:**
     - Title: Required, max 100 characters.
     - Description: Required, max 500 characters.
     - Location: Required, string.
     - Price: Required, positive number.
     - Amenities: Array of strings, optional.
     - Availability: Required, startDate must precede endDate.

2. **PUT** `/api/properties/:id`

   - **Input:**
     ```json
     {
       "price": 150,
       "availability": {
         "startDate": "2024-06-01",
         "endDate": "2024-12-31"
       }
     }
     ```
   - **Output (Success):**
     ```json
     {
       "message": "Property updated successfully"
     }
     ```

3. **DELETE** `/api/properties/:id`
   - **Output (Success):**
     ```json
     {
       "message": "Property deleted successfully"
     }
     ```

### Performance Criteria

- Listing creation/update: <400ms with image uploads.
- Database query optimization for listing searches.

---

## Feature 3: Booking System

**Objective:** Enable guests to book properties and manage their reservations.

### API Endpoints

1. **POST** `/api/bookings`

   - **Input:**
     ```json
     {
       "propertyId": "98765",
       "guestId": "12345",
       "startDate": "2024-03-01",
       "endDate": "2024-03-05"
     }
     ```
   - **Output (Success):**
     ```json
     {
       "message": "Booking confirmed",
       "bookingId": "56789"
     }
     ```
   - **Validation Rules:**
     - Property must be available for the selected dates.
     - Start date must not be in the past.

2. **DELETE** `/api/bookings/:id`

   - **Output (Success):**
     ```json
     {
       "message": "Booking canceled successfully"
     }
     ```

3. **GET** `/api/bookings/:guestId`
   - **Output:**
     ```json
     [
       {
         "bookingId": "56789",
         "propertyId": "98765",
         "startDate": "2024-03-01",
         "endDate": "2024-03-05",
         "status": "confirmed"
       }
     ]
     ```

### Performance Criteria

- Prevent double bookings by validating availability within 200ms.
- Response time: <500ms for fetching bookings.

---

These specifications ensure that the backend supports secure and efficient operations for core functionalities.
