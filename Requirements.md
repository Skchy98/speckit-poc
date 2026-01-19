# Bus Ticket Reservation System - Product Requirements Document

## 1. Executive Summary
The Bus Ticket Reservation System is a web-based platform that allows customers to search,
book, and manage bus tickets online. The system will handle multiple bus operators, routes,
schedules, and real-time seat availability.

## 2. Business Objectives
- Enable online bus ticket booking 24/7
- Reduce manual booking errors
- Provide real-time seat availability
- Generate automated booking confirmations
- Track revenue and booking analytics
- Support multiple payment methods

## 3. User Roles

### 3.1 Customer
- Search for available buses
- Book tickets
- View booking history
- Cancel bookings
- Download tickets
- Provide feedback/ratings

### 3.2 Bus Operator
- Manage bus fleet
- Add/update routes and schedules
- View bookings
- Manage pricing
- View revenue reports

### 3.3 Administrator
- Manage all operators
- Manage routes and cities
- View system analytics
- Handle refunds and disputes
- Manage user accounts

## 4. Core Entities

### 4.1 User
- User ID (auto-generated)
- Full Name
- Email (unique)
- Phone Number
- Password (encrypted)
- Role (Customer/Operator/Admin)
- Created Date
- Last Login
- Status (Active/Inactive)

### 4.2 Bus
- Bus ID (auto-generated)
- Bus Number (unique)
- Bus Type (AC/Non-AC, Sleeper/Seater)
- Operator ID (foreign key)
- Total Seats
- Amenities (WiFi, Charging, Water, etc.)
- Registration Number
- Status (Active/Maintenance/Inactive)

### 4.3 Route
- Route ID (auto-generated)
- Source City
- Destination City
- Distance (in km)
- Estimated Duration
- Status (Active/Inactive)

### 4.4 Schedule
- Schedule ID (auto-generated)
- Bus ID (foreign key)
- Route ID (foreign key)
- Departure Time
- Arrival Time
- Days of Operation (Mon-Sun)
- Base Price
- Valid From Date
- Valid To Date
- Status (Active/Inactive)

### 4.5 Seat
- Seat ID (auto-generated)
- Bus ID (foreign key)
- Seat Number
- Seat Type (Window/Aisle/Middle)
- Floor Level (Lower/Upper for double-decker)
- Status (Available/Blocked)

### 4.6 Booking
- Booking ID (auto-generated, format: BK-YYYYMMDD-XXXX)
- User ID (foreign key)
- Schedule ID (foreign key)
- Journey Date
- Passenger Details (JSON array)
- Seat Numbers (array)
- Total Amount
- Booking Date/Time
- Booking Status (Confirmed/Cancelled/Pending)
- Payment Status (Paid/Pending/Refunded)
- Cancellation Date (if applicable)
- Refund Amount (if applicable)

### 4.7 Passenger
- Passenger ID (auto-generated)
- Booking ID (foreign key)
- Name
- Age
- Gender
- Seat Number
- ID Proof Type
- ID Proof Number

### 4.8 Payment
- Payment ID (auto-generated)
- Booking ID (foreign key)
- Amount
- Payment Method (Credit Card/Debit Card/UPI/Wallet)
- Transaction ID (from payment gateway)
- Payment Date/Time
- Status (Success/Failed/Pending)

### 4.9 City
- City ID (auto-generated)
- City Name
- State
- Country
- Status (Active/Inactive)

### 4.10 Operator
- Operator ID (auto-generated)
- Operator Name
- Contact Email
- Contact Phone
- Address
- Registration Number
- Commission Percentage
- Status (Active/Inactive)

## 5. Functional Requirements

### 5.1 User Management
- FR-1.1: System shall allow user registration with email verification
- FR-1.2: System shall authenticate users with email and password
- FR-1.3: System shall support password reset via email
- FR-1.4: System shall maintain user profiles
- FR-1.5: System shall support role-based access control

### 5.2 Bus Search and Booking
- FR-2.1: Users shall search buses by source, destination, and date
- FR-2.2: System shall display available buses with:
    - Bus name and type
    - Departure and arrival times
    - Available seats count
    - Price
    - Amenities
    - Ratings
- FR-2.3: Users shall filter results by:
    - Bus type (AC/Non-AC)
    - Departure time slots
    - Price range
    - Ratings
    - Amenities
- FR-2.4: Users shall sort results by price, duration, departure time, ratings
- FR-2.5: Users shall view seat layout and select specific seats
- FR-2.6: System shall block selected seats for 10 minutes during booking
- FR-2.7: Users shall enter passenger details for each seat
- FR-2.8: System shall validate passenger information
- FR-2.9: System shall calculate total fare including taxes
- FR-2.10: Users shall complete payment to confirm booking

### 5.3 Booking Management
- FR-3.1: System shall generate unique booking ID
- FR-3.2: System shall send booking confirmation via email and SMS
- FR-3.3: Users shall view all their bookings
- FR-3.4: Users shall download/print tickets
- FR-3.5: Users shall cancel bookings (before 2 hours of departure)
- FR-3.6: System shall calculate refund based on cancellation policy:
    - More than 24 hours: 90% refund
    - 12-24 hours: 70% refund
    - 2-12 hours: 50% refund
    - Less than 2 hours: No refund
- FR-3.7: System shall process refunds within 5-7 business days

### 5.4 Operator Management
- FR-4.1: Operators shall add/update bus information
- FR-4.2: Operators shall create and manage schedules
- FR-4.3: Operators shall set pricing for different dates
- FR-4.4: Operators shall view all bookings for their buses
- FR-4.5: Operators shall view revenue reports
- FR-4.6: Operators shall mark buses as unavailable for maintenance

### 5.5 Route Management
- FR-5.1: Admin shall add new routes with source and destination
- FR-5.2: Admin shall manage cities database
- FR-5.3: System shall prevent duplicate routes
- FR-5.4: Admin shall activate/deactivate routes

### 5.6 Payment Processing
- FR-6.1: System shall integrate with payment gateway
- FR-6.2: System shall support multiple payment methods
- FR-6.3: System shall handle payment failures gracefully
- FR-6.4: System shall store payment transaction details
- FR-6.5: System shall generate payment receipts

### 5.7 Reporting and Analytics
- FR-7.1: System shall generate booking reports
- FR-7.2: System shall track revenue by operator
- FR-7.3: System shall show popular routes
- FR-7.4: System shall display occupancy rates
- FR-7.5: Admin shall export reports in CSV/PDF format

### 5.8 Notifications
- FR-8.1: System shall send booking confirmation email/SMS
- FR-8.2: System shall send journey reminder 24 hours before
- FR-8.3: System shall notify cancellations
- FR-8.4: System shall send payment receipts

## 6. Non-Functional Requirements

### 6.1 Performance
- NFR-1.1: Search results shall load within 2 seconds
- NFR-1.2: System shall support 1000 concurrent users
- NFR-1.3: Booking process shall complete within 5 seconds

### 6.2 Security
- NFR-2.1: All passwords shall be encrypted using bcrypt
- NFR-2.2: All API endpoints shall use HTTPS
- NFR-2.3: Payment information shall be PCI-DSS compliant
- NFR-2.4: System shall implement JWT-based authentication
- NFR-2.5: System shall prevent SQL injection attacks

### 6.3 Availability
- NFR-3.1: System shall have 99.5% uptime
- NFR-3.2: System shall have automated backups daily

### 6.4 Scalability
- NFR-4.1: Database shall handle up to 1 million bookings
- NFR-4.2: System shall be horizontally scalable

## 7. API Endpoints Required

### Authentication
- POST /api/auth/register
- POST /api/auth/login
- POST /api/auth/logout
- POST /api/auth/forgot-password
- POST /api/auth/reset-password

### User Management
- GET /api/users/profile
- PUT /api/users/profile
- GET /api/users/{id}
- PUT /api/users/{id}
- DELETE /api/users/{id}

### City Management
- GET /api/cities
- POST /api/cities
- GET /api/cities/{id}
- PUT /api/cities/{id}
- DELETE /api/cities/{id}

### Bus Management
- GET /api/buses
- POST /api/buses
- GET /api/buses/{id}
- PUT /api/buses/{id}
- DELETE /api/buses/{id}
- GET /api/buses/operator/{operatorId}

### Route Management
- GET /api/routes
- POST /api/routes
- GET /api/routes/{id}
- PUT /api/routes/{id}
- DELETE /api/routes/{id}

### Schedule Management
- GET /api/schedules
- POST /api/schedules
- GET /api/schedules/{id}
- PUT /api/schedules/{id}
- DELETE /api/schedules/{id}
- GET /api/schedules/bus/{busId}

### Search and Availability
- GET /api/search?source={city}&destination={city}&date={date}
- GET /api/schedules/{scheduleId}/seats?date={date}

### Booking Management
- POST /api/bookings
- GET /api/bookings/{id}
- GET /api/bookings/user/{userId}
- PUT /api/bookings/{id}/cancel
- GET /api/bookings/{id}/ticket

### Payment
- POST /api/payments/initiate
- POST /api/payments/confirm
- GET /api/payments/{id}

### Operator Management
- GET /api/operators
- POST /api/operators
- GET /api/operators/{id}
- PUT /api/operators/{id}
- DELETE /api/operators/{id}

### Reports
- GET /api/reports/bookings
- GET /api/reports/revenue
- GET /api/reports/popular-routes

## 8. Database Constraints
- Email must be unique across users
- Bus numbers must be unique per operator
- Booking ID must be unique and auto-generated
- Seats cannot be double-booked for same schedule and date
- Cancellation allowed only before 2 hours of departure
- Payment must be completed before booking confirmation

## 9. Business Rules
- BR-1: Maximum 6 tickets per booking
- BR-2: Seat blocking timeout: 10 minutes
- BR-3: Booking opens 30 days in advance
- BR-4: Booking closes 30 minutes before departure
- BR-5: Cancellation charges as per policy
- BR-6: Children below 5 years travel free (no seat)
- BR-7: Senior citizens get 10% discount
- BR-8: Operator commission: 15% of ticket price

## 10. Success Criteria
- 1000+ bookings per month
- Average booking time < 3 minutes
- Customer satisfaction rating > 4.0/5.0
- System uptime > 99.5%
- Payment success rate > 95%
