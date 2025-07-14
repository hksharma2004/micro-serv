# Microservices Architecture Project

A robust microservices-based application featuring user management, ride booking, and captain services with asynchronous communication using RabbitMQ.

## 🏗️ Architecture Overview

This project implements a microservices architecture with the following services:

- **Gateway Service**: API Gateway handling routing and request proxying
- **User Service**: User authentication, registration, and profile management
- **Captain Service**: Captain management with long polling for ride requests
- **Ride Service**: Ride booking and management functionality

## 🚀 Key Features

### Authentication & Security
- **JWT (JSON Web Tokens)**: Stateless authentication across all services
- **bcrypt**: Secure password hashing and validation
- **Token Blacklisting**: Invalidate tokens on logout for enhanced security
- **Cookie Parser**: Handle authentication cookies seamlessly

### Microservices Communication
- **RabbitMQ**: Asynchronous message queuing between services
- **CloudAMQP**: Cloud-hosted RabbitMQ management
- **Long Polling**: Real-time captain ride request notifications
- **HTTP Proxy**: Service-to-service communication through the gateway

### Core Technologies
- **Express.js**: Web application framework for all services
- **Express Router**: Modular route handling
- **dotenv**: Environment variable management
- **Axios**: HTTP client for inter-service communication

## 📋 Services Breakdown

### Gateway Service
- **Role**: Entry point for all client requests
- **Technologies**: Express, HTTP Proxy, Express HTTP Proxy
- **Features**: 
  - Request routing to appropriate microservices
  - Load balancing
  - Authentication middleware

### User Service
- **Role**: Handle user authentication and profile management
- **Technologies**: Express, JWT, bcrypt, Cookie Parser
- **Features**:
  - User registration and login
  - JWT token generation and validation
  - Password encryption
  - Token blacklisting on logout

### Captain Service
- **Role**: Manage captain operations and ride assignments
- **Technologies**: Express, Axios, RabbitMQ (amqplib)
- **Features**:
  - Captain registration and authentication
  - **Long Polling**: Continuous polling for new ride requests
  - Real-time ride assignment notifications
  - Integration with ride service via message queue

### Ride Service
- **Role**: Handle ride booking and management
- **Technologies**: Express, RabbitMQ (amqplib)
- **Features**:
  - Ride creation and booking
  - Publish ride requests to message queue
  - Real-time ride status updates

## 🔄 RabbitMQ Integration

### CloudAMQP Manager Functions
The project uses CloudAMQP's RabbitMQ service with the following exported functions:

```javascript
// RabbitMQ Manager Exports
{
  subscribeToQueue,    // Subscribe to receive messages from a queue
  publishToQueue,      // Publish messages to a queue
  connect             // Establish connection to RabbitMQ
}
```

### Asynchronous Communication Flow
1. **Ride Request**: User creates a ride request
2. **Queue Publishing**: Ride service publishes request to RabbitMQ queue
3. **Captain Polling**: Captain service uses long polling to check for new rides
4. **Message Consumption**: Captain service subscribes to ride queue
5. **Real-time Updates**: Available captains receive ride notifications instantly

## 🛠️ Technology Stack

| Technology | Purpose | Usage |
|------------|---------|-------|
| **Express.js** | Web Framework | All services use Express for HTTP handling |
| **RabbitMQ (amqplib)** | Message Queue | Asynchronous communication between services |
| **JWT** | Authentication | Stateless token-based authentication |
| **bcrypt** | Password Security | Hash and validate user passwords |
| **Cookie Parser** | Cookie Handling | Parse and manage authentication cookies |
| **HTTP Proxy** | Request Routing | Proxy requests through gateway |
| **Express HTTP Proxy** | Service Routing | Route requests to appropriate microservices |
| **dotenv** | Configuration | Environment variable management |
| **Axios** | HTTP Client | Inter-service communication |
| **CloudAMQP** | Message Broker | Cloud-hosted RabbitMQ service |

## 🔐 Security Features

### Token Management
- **JWT Generation**: Secure token creation with expiration
- **Token Validation**: Middleware for route protection
- **Token Blacklisting**: Maintain blacklist of invalidated tokens
- **Secure Cookies**: HttpOnly cookies for token storage

### Password Security
- **bcrypt Hashing**: Industry-standard password encryption
- **Salt Rounds**: Configurable hashing complexity
- **Password Validation**: Secure password comparison

## 🔄 Long Polling Implementation

### Captain Service Polling
The captain service implements long polling to efficiently handle ride requests:

```javascript
// Long polling for new ride requests
async function pollForRides() {
  while (true) {
    try {
      const response = await axios.get('/api/rides/poll', {
        timeout: 30000 // 30 second timeout
      });
      
      if (response.data.newRide) {
        // Process new ride data
        handleNewRide(response.data);
      }
    } catch (error) {
      // Handle polling errors
      console.error('Polling error:', error);
    }
  }
}
```

## 📦 Installation & Setup

1. Clone the repository
2. Install dependencies for each service:
   ```bash
   cd gateway && npm install
   cd ../user && npm install
   cd ../captain && npm install
   cd ../ride && npm install
   ```
3. Set up environment variables in each service's `.env` file
4. Configure CloudAMQP connection string
5. Start services in development mode

## 🌐 API Gateway Pattern

The gateway service acts as a single entry point, providing:
- **Unified API**: Single endpoint for client applications
- **Request Routing**: Intelligent routing to appropriate services
- **Authentication**: Centralized authentication handling
- **Load Balancing**: Distribute requests across service instances

## 📊 Message Queue Architecture

```
User Service → Ride Service → RabbitMQ → Captain Service
     ↓              ↓            ↓            ↓
  Auth Tokens → Ride Requests → Queues → Long Polling
```

## 🔧 Environment Variables

Each service requires specific environment variables:
- Database connection strings
- JWT secret keys
- RabbitMQ connection URLs
- Service port configurations
- CloudAMQP credentials

## 🚦 Getting Started

1. Ensure RabbitMQ is running (or CloudAMQP is configured)
2. Start services in the following order:
   - Gateway Service
   - User Service
   - Ride Service
   - Captain Service
3. Access the application through the gateway service endpoint

## 📈 Future Enhancements

- Service discovery and registration
- Circuit breaker pattern implementation
- Distributed tracing
- Kubernetes deployment configuration
- API rate limiting and throttling

---

*This microservices architecture provides a scalable, maintainable, and efficient solution for ride-sharing applications with real-time communication capabilities.*