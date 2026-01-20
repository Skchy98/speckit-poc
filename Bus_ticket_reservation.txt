Step-by-Step POC Setup
Step 1: Install Prerequisites
bash# Check Python version (requires 3.11+)
python3 --version

# Check if uv is installed
uv --version

# If uv not installed, install it
curl -LsSf https://astral.sh/uv/install.sh | sh

# Verify installation
uv --version
Step 2: Install Specify CLI (One-Time with uvx)
Since you want to use uvx for one-time setup:
bash# One-time usage without installation
uvx --from git+https://github.com/github/spec-kit.git specify init bus-reservation-system
Or for persistent installation (recommended):
bash# Install once and use everywhere
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

# Verify installation
specify --help
Step 3: Initialize Your Project
bash# Using persistent installation
specify init bus-reservation-system --ai claude

# OR using one-time uvx
uvx --from git+https://github.com/github/spec-kit.git specify init bus-reservation-system --ai claude

# For other AI assistants:
# --ai copilot (GitHub Copilot)
# --ai cursor-agent (Cursor)
# --ai gemini (Gemini CLI)
# --ai qoder (Qoder CLI)
```

**What this creates:**
```
bus-reservation-system/
├── .specify/
│   ├── memory/
│   │   └── constitution.md           # Project principles (created in next step)
│   ├── scripts/
│   │   ├── check-prerequisites.sh
│   │   ├── common.sh
│   │   ├── create-new-feature.sh
│   │   ├── setup-plan.sh
│   │   └── update-claude-md.sh
│   └── templates/
│       ├── plan-template.md
│       ├── spec-template.md
│       └── tasks-template.md
└── .github/
└── (AI agent specific files)
Step 4: Navigate to Project and Launch AI Agent
bashcd bus-reservation-system

# Launch your AI agent
claude                    # For Claude Code
# OR
cursor .                  # For Cursor
# OR
code .                    # For VS Code with Copilot
```

You should now see these slash commands available in your AI agent:
- `/speckit.constitution`
- `/speckit.specify`
- `/speckit.plan`
- `/speckit.tasks`
- `/speckit.implement`
- `/speckit.clarify`
- `/speckit.analyze`
- `/speckit.checklist`

---

## The Spec-Driven Development Workflow

### PHASE 1: Establish Project Constitution

This defines the non-negotiable principles for your project.

**Command:**
```
/speckit.constitution
```

**Prompt to use:**
```
Create project governing principles for a bus reservation system with the following focus areas:

1. Code Quality & Architecture:
    - Use Java 21 with Spring Boot 3.3
    - Follow clean architecture principles
    - Implement repository pattern for data access
    - Use DTO pattern for API responses

2. Testing Standards:
    - Minimum 80% code coverage
    - Unit tests for all service layer methods
    - Integration tests for all REST endpoints
    - Use H2 for test database

3. Security & Compliance:
    - All passwords must be encrypted with BCrypt
    - JWT-based authentication required
    - HTTPS only for production
    - PCI-DSS compliance for payment data

4. Database Standards:
    - Use MySQL 8.0+
    - All tables must have proper indexes
    - Use soft deletes where applicable
    - Implement audit trails for bookings

5. API Design:
    - RESTful design following OpenAPI 3.0
    - Consistent error response format
    - Use proper HTTP status codes
    - Include Swagger documentation

6. Performance:
    - API response time < 2 seconds
    - Support for 1000 concurrent users
    - Implement caching for frequently accessed data
```

**What this creates:**
- `.specify/memory/constitution.md` with your project principles

---

### PHASE 2: Create Functional Specification

Now you define WHAT you want to build, focusing on features and requirements, NOT technology.

**Command:**
```
/speckit.specify
```

**Prompt to use (copy your PM requirements):**
```
Build a Bus Ticket Reservation System with the following capabilities:

## Business Overview
An online platform for customers to search, book, and manage bus tickets. The system handles multiple bus operators, routes, schedules, and real-time seat availability.

## User Roles
1. Customer - search buses, book tickets, view history, cancel bookings
2. Bus Operator - manage fleet, routes, schedules, pricing, view revenue
3. Administrator - manage operators, routes, cities, analytics, handle refunds

## Core Features

### User Management
- User registration with email verification
- Login with email and password
- Password reset functionality
- Role-based access control (Customer/Operator/Admin)
- User profile management

### Bus Search & Booking
- Search buses by source city, destination city, and travel date
- Display available buses with:
    - Bus details (name, type, amenities)
    - Departure and arrival times
    - Available seats count
    - Price and ratings
- Filter results by bus type, time slots, price range, amenities, ratings
- Sort by price, duration, departure time, ratings
- View seat layout and select specific seats
- Seat blocking for 10 minutes during booking process
- Enter passenger details for each seat
- Calculate total fare with taxes
- Payment processing to confirm booking

### Booking Management
- Generate unique booking ID (format: BK-YYYYMMDD-XXXX)
- Send booking confirmation via email and SMS
- View booking history
- Download/print tickets
- Cancel bookings (before 2 hours of departure)
- Refund calculation:
    - >24 hours before: 90% refund
    - 12-24 hours: 70% refund
    - 2-12 hours: 50% refund
    - <2 hours: No refund

### Operator Features
- Add and manage bus information
- Create and manage schedules
- Set pricing for different dates
- View bookings for their buses
- View revenue reports
- Mark buses as unavailable for maintenance

### Route & City Management
- Add routes with source and destination
- Manage cities database
- Prevent duplicate routes
- Activate/deactivate routes

### Payment Processing
- Support multiple payment methods (Credit Card, Debit Card, UPI, Wallet)
- Handle payment failures gracefully
- Store transaction details securely
- Generate payment receipts

### Reporting & Analytics
- Booking reports
- Revenue tracking by operator
- Popular routes analysis
- Occupancy rates
- Export reports in CSV/PDF

## Business Rules
- Maximum 6 tickets per booking
- Seat blocking timeout: 10 minutes
- Booking opens 30 days in advance
- Booking closes 30 minutes before departure
- Children below 5 years travel free (no seat)
- Senior citizens get 10% discount
- Operator commission: 15% of ticket price

## Success Criteria
- System handles 1000+ bookings per month
- Average booking time < 3 minutes
- Customer satisfaction > 4.0/5.0
- System uptime > 99.5%
- Payment success rate > 95%
```

**What happens:**
- AI agent creates a new Git branch (e.g., `001-bus-reservation-core`)
- Creates `.specify/specs/001-bus-reservation-core/spec.md`
- Spec includes user stories, functional requirements, and acceptance criteria

---

### PHASE 3: Clarify Requirements (IMPORTANT!)

You should run the structured clarification workflow before creating a technical plan to reduce rework downstream .

**Command:**
```
/speckit.clarify
```

The AI will ask targeted questions about unclear areas. Answer them to refine the spec.

**Then validate with:**
```
Read the review and acceptance checklist, and check off each item if the feature spec meets the criteria. Leave it empty if it does not.
```

---

### PHASE 4: Generate Technical Plan

Now you specify HOW to build it - the tech stack and architecture.

**Command:**
```
/speckit.plan
```

**Prompt to use:**
```
Create a technical implementation plan for the Bus Reservation System using:

## Technology Stack
- Java 21 as the programming language
- Spring Boot 3.3.0 for the backend framework
- MySQL 8.0+ for the database
- Spring Data JPA for ORM
- Spring Security with JWT for authentication
- Hibernate Validator for data validation
- Lombok for reducing boilerplate code
- MapStruct for DTO mapping
- Springdoc OpenAPI for API documentation
- Maven for build management

## Architecture
- Clean architecture with clear separation of concerns
- Layered architecture:
    - Controller layer (REST endpoints)
    - Service layer (business logic)
    - Repository layer (data access)
    - Entity layer (domain models)
    - DTO layer (data transfer objects)

## Database Design
- Use JPA entities with proper relationships
- Implement indexes on frequently queried columns
- Use appropriate data types and constraints
- Implement soft deletes using @SQLDelete annotation
- Add audit columns (createdDate, modifiedDate)

## API Design
- RESTful API following best practices
- Standard HTTP methods (GET, POST, PUT, DELETE)
- Consistent URL patterns (/api/v1/resource)
- Proper use of HTTP status codes
- Standardized error response format
- Request/response validation
- Swagger/OpenAPI documentation at /swagger-ui.html

## Security
- BCrypt for password hashing
- JWT tokens for authentication
- Role-based authorization using @PreAuthorize
- CORS configuration for frontend integration
- Input validation and sanitization

## Additional Features
- Global exception handling
- Logging with SLF4J and Logback
- Application properties for configuration
- Profile-based configuration (dev, prod)

## Entities to Create
1. User (id, fullName, email, phoneNumber, password, role, status)
2. City (id, cityName, state, country, status)
3. Operator (id, operatorName, contactEmail, contactPhone, registrationNumber)
4. Bus (id, busNumber, busType, operatorId, totalSeats, amenities)
5. Route (id, sourceCityId, destinationCityId, distance, duration)
6. Schedule (id, busId, routeId, departureTime, arrivalTime, daysOfOperation, basePrice)
7. Seat (id, busId, seatNumber, seatType, floorLevel)
8. Booking (id, bookingId, userId, scheduleId, journeyDate, seatNumbers, totalAmount, status)
9. Passenger (id, bookingId, name, age, gender, seatNumber)
10. Payment (id, bookingId, amount, paymentMethod, transactionId, status)

## REST Endpoints Required
Authentication:
- POST /api/auth/register
- POST /api/auth/login
- POST /api/auth/logout
- POST /api/auth/forgot-password

Buses:
- GET /api/buses
- POST /api/buses
- GET /api/buses/{id}
- PUT /api/buses/{id}
- DELETE /api/buses/{id}

Search:
- GET /api/search?source={city}&destination={city}&date={date}
- GET /api/schedules/{scheduleId}/seats?date={date}

Bookings:
- POST /api/bookings
- GET /api/bookings/{id}
- GET /api/bookings/user/{userId}
- PUT /api/bookings/{id}/cancel

And all other CRUD endpoints as needed.
```

**What this creates:**
```
.specify/specs/001-bus-reservation-core/
├── spec.md
├── plan.md                  # Overall implementation strategy
├── data-model.md            # Database schema and relationships
├── contracts/
│   └── api-spec.json        # OpenAPI specification
├── quickstart.md            # Setup and run instructions
└── research.md              # Technology choices and rationale
```

---

### PHASE 5: Validate the Plan

Ask the AI to audit the plan:
```
Review the implementation plan and implementation detail files. Check for:
1. Are all entities properly defined with relationships?
2. Are all REST endpoints mapped to user stories?
3. Is the database schema complete?
4. Are there any missing pieces in the architecture?
5. Is the security implementation comprehensive?

For each section of the plan, reference specific implementation details and confirm completeness.
```

---

### PHASE 6: Generate Task Breakdown

Break down the plan into actionable tasks.

**Command:**
```
/speckit.tasks
```

**What this creates:**
```
.specify/specs/001-bus-reservation-core/tasks.md


...................................................


# Access Swagger UI
open http://localhost:8080/swagger-ui.html

# Test registration
curl -X POST http://localhost:8080/api/auth/register \
-H "Content-Type: application/json" \
-d '{
"fullName": "John Doe",
"email": "john@example.com",
"phoneNumber": "1234567890",
"password": "password123",
"role": "CUSTOMER"
}'
```

---

## Key Commands Summary

| Command | Purpose | When to Use |
|---------|---------|-------------|
| `/speckit.constitution` | Define project principles | First step - sets guardrails |
| `/speckit.specify` | Create functional spec | Define WHAT to build |
| `/speckit.clarify` | Ask clarifying questions | Before planning - fill gaps |
| `/speckit.plan` | Create technical plan | Define HOW to build |
| `/speckit.tasks` | Generate task breakdown | Before implementation |
| `/speckit.implement` | Execute all tasks | Build the application |
| `/speckit.analyze` | Cross-check consistency | Optional - validate quality |
| `/speckit.checklist` | Generate quality checklist | Optional - extra validation |

---

## Directory Structure After Full Workflow
```
bus-reservation-system/
├── .specify/
│   ├── memory/
│   │   └── constitution.md
│   ├── specs/
│   │   └── 001-bus-reservation-core/
│   │       ├── spec.md
│   │       ├── plan.md
│   │       ├── data-model.md
│   │       ├── tasks.md
│   │       ├── quickstart.md
│   │       ├── research.md
│   │       └── contracts/
│   │           └── api-spec.json
│   └── templates/
├── src/
│   ├── main/
│   │   ├── java/com/busreservation/
│   │   │   ├── BusReservationApplication.java
│   │   │   ├── entity/
│   │   │   ├── repository/
│   │   │   ├── service/
│   │   │   ├── controller/
│   │   │   ├── dto/
│   │   │   ├── config/
│   │   │   ├── exception/
│   │   │   └── security/
│   │   └── resources/
│   │       └── application.properties
│   └── test/
├── pom.xml
└── README.md

..................................................

Contains:

Tasks organized by user story
File paths for each implementation
Dependencies between tasks
Parallel execution markers [P]
Test tasks if TDD is enabled
Checkpoint validations

Example structure:
markdown## Phase 1: Database Setup
- [ ] 1.1 Create User entity with JPA annotations
- [ ] 1.2 Create City entity
- [ ] 1.3 Create Operator entity
- [ ] 1.4 Create Bus entity with relationships
  ...

## Phase 2: Repository Layer
- [ ] 2.1 Create UserRepository interface
- [ ] 2.2 Create CityRepository interface
  ...

## Phase 3: Service Layer
- [ ] 3.1 Implement UserService
- [ ] 3.2 Implement AuthenticationService
  ...
```

---

### PHASE 7: Implementation

Now the AI executes all the tasks automatically!

**Command:**
```
/speckit.implement
What happens:

AI reads tasks.md
Executes each task in order
Respects dependencies
Runs parallel tasks when possible
Creates all files, entities, repositories, services, controllers
Adds tests if specified
Provides progress updates

The AI will:

Create project structure
Generate pom.xml with all dependencies
Create application.properties
Generate all entity classes
Create repository interfaces
Implement service layer
Build REST controllers
Add exception handlers
Configure security
Generate OpenAPI documentation

.................................