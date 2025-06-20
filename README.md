# airbnb-clone-project
## Team Roles

This project was developed collaboratively by our team. Below are the assigned roles and responsibilities for each member:

-**Backend Developer:**
Develops server-side logic, manages the database, and ensures API functionality and security.

-**Database Administrator:**
Designs and manages the database structure, ensures data integrity and security, and handles performance optimization.

-**DevOps Engineer:**
Manages deployment pipelines, automates builds and testing, maintains CI/CD workflows, and monitors infrastructure and application health.

-**QA Engineer:**
Plans and executes test cases, identifies bugs, ensures code quality, and verifies that all features meet functional and performance requirements.

> All team members contribute to testing, code reviews, and collaborative decision-making.

## Technology Stack

Below is a list of the technologies used in this project, along with a brief explanation of their purpose and role:

-**Django:**
A high-level Python web framework used for building robust backends and RESTful APIs efficiently, with built-in admin functionality and ORM support.

-**Django REST Framework:**
 A powerful and flexible toolkit built on top of Django, used to build Web APIs. It simplifies the process of creating RESTful endpoints for the application, including serialization, authentication, and permissions.

 - **PostgreSQL:**  
  A powerful, open-source relational database used to store and manage structured data with support for advanced queries and transactions.

- **GraphQL:**  
  A query language for APIs that allows clients to request only the data they need. It improves efficiency and flexibility compared to traditional REST APIs, and is used to serve complex frontend data needs.

  - **Celery:**  
  An asynchronous task queue used to run background jobs such as sending emails, processing long-running tasks, and scheduling periodic tasks. It integrates seamlessly with Django and improves the responsiveness of the application.

- **Redis:**  
  An in-memory data store used as a message broker for Celery. It enables fast and reliable task queuing and also supports caching for performance optimization.

  - **Docker:**  
  A containerization platform that packages the application and its dependencies into isolated environments, ensuring consistency across development, testing, and production.

- **CI/CD Pipelines:**  
  Continuous Integration and Continuous Deployment tools used to automate testing, building, and deployment processes, ensuring code quality and faster delivery.

> This technology stack allows us to build a modern, scalable, and maintainable web application with both REST and GraphQL APIs, asynchronous processing, automated deployment, and a responsive frontend.

## Database Design

The following is an overview of the main database entities in the project, along with key fields and their relationships:

### 1. Users
Represents individuals using the platform, both guests and hosts.
- `id`: Primary key
- `username`: Unique identifier for login
- `email`: Contact information
- `password`: Hashed for authentication
- `role`: Defines if the user is a guest, host, or admin

**Relationships**:  
- A user can own multiple properties.  
- A user can make multiple bookings.  
- A user can leave multiple reviews.  
- A user can make multiple payments.

---

### 2. Properties
Represents real estate listings that can be booked.
- `id`: Primary key
- `owner_id`: Foreign key to Users (host)
- `title`: Name or short description
- `location`: Address or geographic info
- `price_per_night`: Cost of booking per night

**Relationships**:  
- A property belongs to one user (host).  
- A property can have multiple bookings.  
- A property can have multiple reviews.

---

### 3. Bookings
Tracks reservations made by users for properties.
- `id`: Primary key
- `user_id`: Foreign key to Users (guest)
- `property_id`: Foreign key to Properties
- `start_date`: Booking start
- `end_date`: Booking end
- `status`: e.g., confirmed, cancelled, completed

**Relationships**:  
- A booking is made by one user.  
- A booking is for one property.  
- A booking can have one associated payment.

---

### 4. Reviews
Captures user feedback for properties.
- `id`: Primary key
- `user_id`: Foreign key to Users (guest)
- `property_id`: Foreign key to Properties
- `rating`: Numerical rating (e.g., 1-5)
- `comment`: Text feedback

**Relationships**:  
- A review is written by one user.  
- A review is for one property.

---

### 5. Payments
Tracks financial transactions for bookings.
- `id`: Primary key
- `booking_id`: Foreign key to Bookings
- `amount`: Total cost paid
- `payment_date`: When the transaction occurred
- `payment_method`: e.g., credit card, PayPal

**Relationships**:  
- A payment belongs to one booking.  
- A booking can have one payment.

---

> This relational model supports key platform functions, including listing properties, managing reservations, collecting payments, and gathering user feedback.

## Feature Breakdown

> This section covers the main backend features of the application, including available APIs, user functionality, and improvements in backend performance.

### 1. API Documentation
- **OpenAPI Standard**: The backend APIs are documented using the OpenAPI (Swagger) standard. This ensures clarity, consistency, and easy integration for frontend or external developers.
- **Django REST Framework (DRF)**: Exposes RESTful endpoints to support CRUD operations on users, properties, bookings, and more.
- **GraphQL**: Offers a flexible and efficient way to query and manipulate data, allowing clients to request exactly what they need.

### 2. User Authentication
- **Endpoints**: `/users/`, `/users/{user_id}/`
- **Features**: Register new users, log in, manage authentication tokens, and update user profile data. Supports both guest and host roles.

### 3. Property Management
- **Endpoints**: `/properties/`, `/properties/{property_id}/`
- **Features**: Hosts can create, retrieve, update, and delete property listings. Listings include details such as price, location, and amenities.

### 4. Booking System
- **Endpoints**: `/bookings/`, `/bookings/{booking_id}/`
- **Features**: Guests can create bookings for properties, update reservation details, and manage check-in and check-out status.

### 5. Payment Processing
- **Endpoints**: `/payments/`
- **Features**: Handles all payment transactions securely. It integrates with bookings to record payment status, amount, and timestamps.

### 6. Review System
- **Endpoints**: `/reviews/`, `/reviews/{review_id}/`
- **Features**: Users can post and manage reviews for the properties they have booked. Each review includes a rating and an optional comment.

### 7. Database Optimizations
- **Indexing**: Frequently queried fields, such as user ID and property ID, are indexed to improve data retrieval speed.
- **Caching**: Uses caching strategies, including Redis, to reduce load on the database and speed up common queries and API responses.

> Together, these features make the system efficient, scalable, and user-friendly, supporting both traditional RESTful API workflows and modern GraphQL clients.

## API Security

> Security is a key part of this application. It protects user data, ensures transaction integrity, and stops unauthorized access. The following measures will help secure the API:

### 1. Authentication
- **Method**: Token-based authentication using JWT (JSON Web Tokens) or session-based authentication.
- **Purpose**: Verifies that only confirmed users can access protected endpoints, like booking or profile management.
- **Importance**: Stops unauthorized access to sensitive user accounts and data.

### 2. Authorization
- **Method**: Role-based access control (RBAC).
- **Purpose**: Limits access to specific actions based on user roles, such as allowing only hosts to create properties.
- **Importance**: Ensures users can only do what they are allowed, supporting business rules and preventing privilege escalation.

### 3. Rate Limiting
- **Tool**: Django Ratelimit or similar middleware.
- **Purpose**: Controls the number of API requests from a single IP or user over a specific time.
- **Importance**: Protects the API from abuse, brute-force attacks, and denial-of-service (DoS) threats.

### 4. Input Validation & Sanitization
- **Method**: Validate all incoming data using serializers and forms.
- **Purpose**: Prevents injection attacks, like SQL injection and XSS, by ensuring only valid data is processed.
- **Importance**: Maintains database integrity and protects users from harmful input.

### 5. Secure Payment Handling
- **Method**: Use HTTPS for all traffic and work with secure third-party payment gateways, such as Stripe.
- **Purpose**: Encrypts payment data in transit and relies on PCI-compliant providers for payment processing.
- **Importance**: Protects financial information and helps prevent fraud.

### 6. HTTPS & SSL
- **Method**: Use SSL certificates and redirect all HTTP requests to HTTPS.
- **Purpose**: Encrypts all data sent between clients and servers.
- **Importance**: Stops man-in-the-middle attacks and prevents eavesdropping on sensitive data.

---

> By implementing these security measures, the application protects personal information, secures transactions, and ensures safe interactions between users and the system.

## CI/CD Pipeline

### What is CI/CD?

**CI/CD** means **Continuous Integration** and **Continuous Deployment/Delivery**. It is a development practice that allows teams to automatically build, test, and deploy code changes more often and reliably. CI makes sure that new code is merged and tested continuously, while CD automates the delivery of that code to production environments.

### Why It Matters for This Project

Implementing a CI/CD pipeline helps:
- Detect bugs early with automated testing
- Lower manual deployment errors
- Ensure consistent and repeatable deployments
- Speed up development by providing quick feedback
- Maintain high code quality and project stability

### Tools Used

- **GitHub Actions**: Automates workflows for building, testing, and deploying the application when code is pushed or merged into the repository.
- **Docker**: Creates containerized environments to ensure consistency across development, testing, and production.
- **Docker Compose**: Helps set up services like the web server, database, and Redis in isolated containers for testing and deployment.
- **Heroku / Render / AWS** *(optional)*: Cloud hosting platforms that can host the application for automated deployment once CI checks pass.
- **pytest / unittest**: Frameworks for writing and running automated tests during the CI stage.

---

> The CI/CD pipeline boosts team productivity and system reliability by automating repetitive tasks and ensuring every code change is checked before release.

## UI/UX Design Planning

### Design Goals

The primary objective of the UI/UX design is to create an intuitive, visually appealing, and seamless experience for users interacting with the property booking system. The design should promote ease of use, enhance engagement, and reduce friction in the property search and booking process. The following goals guide our design approach:

- **Simplicity**: Interface elements should be clean and uncluttered.
- **Responsiveness**: The design must be optimized for all screen sizes (mobile, tablet, desktop).
- **Accessibility**: Ensure the interface is usable by people of all abilities, following accessibility standards.
- **Consistency**: Use uniform styling, fonts, and colors throughout the application.
- **Efficiency**: Enable users to perform tasks (e.g., browse listings, view details, complete booking) with minimal steps.

### Key Features to Implement

- Property listings with filtering and sorting options
- Individual property details page with images, descriptions, and pricing
- Streamlined checkout flow with booking confirmation
- Mobile-first responsive layout
- Easy navigation between views/pages

### Page Descriptions

| Page Name              | Description                                                                                  | Key Elements                                                                                   |
|------------------------|----------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| **Property Listing View**  | Displays all available properties with options to filter or sort based on user preferences. | Grid or card layout, filter bar, search input, pagination or infinite scroll.                 |
| **Listing Detailed View**  | Shows detailed information about a selected property.                                        | Image carousel, property description, pricing, availability calendar, "Book Now" button.      |
| **Simple Checkout View**   | Allows users to finalize the booking with minimal steps.                                     | Form with user details, payment summary, confirmation button, and success message post-checkout.|

### Importance of User-Friendly Design in a Booking System

A user-friendly interface is critical in a booking system for several reasons:

- **Reduces Drop-Offs**: If users face difficulties while navigating or completing actions, they are likely to abandon the process.
- **Builds Trust**: Clean, professional, and accessible design signals credibility and reliability.
- **Improves Conversion Rates**: An intuitive layout helps guide users to take desired actions like booking or contacting support.
- **Enhances User Satisfaction**: A smooth experience ensures users return and recommend the platform to others.

By prioritizing a strong UI/UX design, the system will not only attract more users but also ensure they can efficiently complete bookings with confidence.
