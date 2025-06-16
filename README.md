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
