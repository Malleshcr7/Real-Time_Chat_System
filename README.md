# Sent Chat

Sent Chat is a full-stack real-time chat application featuring user authentication, friend management, group and private messaging, and a modern responsive UI. The project is built with a Java Spring Boot backend and a React + TypeScript + Vite frontend.

---

## Features

### Authentication
- User registration, login, logout
- Token refresh for session management

### User Profile
- Setup and update profile (name, bio, profile image)
- View own profile information

### Chat & Messaging
- Create, update, and delete chats (with optional chat image)
- List and view chats
- Send and receive messages (text and media)
- Real-time typing indicators
- Mark messages as read

### Friend Management
- Search users and friends
- Send, accept, reject, and cancel friend requests
- List friends, incoming and sent requests
- Block and unblock users
- Remove friends

### Real-Time & Streaming
- WebSocket-based real-time features (typing, messaging)
- Stream token generation for real-time services

### Responsive UI
- Mobile and desktop layouts
- Video call modal and PiP (UI components)

---

## MVP (Minimum Viable Product)

- User authentication (register, login, logout)
- Profile setup and update
- Friend request workflow (send, accept, reject, cancel)
- Block/unblock users
- Create and manage chats
- Send/receive messages (with media)
- Real-time typing indicators
- Responsive chat interface

---

## Project Structure

```
├── client/    # Frontend (React + TypeScript + Vite)
│   ├── src/
│   ├── public/
│   └── ...
├── server/    # Backend (Java Spring Boot)
│   ├── src/
│   └── ...
└── docker-compose.yml
```

---

## Getting Started

### Prerequisites
- Node.js (for frontend)
- Java 17+ and Maven (for backend)
- Docker (optional, for containerized setup)

### Backend Setup
1. Navigate to `server/`
2. Build and run the Spring Boot application:
   ```sh
   ./mvnw spring-boot:run
   ```

### Frontend Setup
1. Navigate to `client/`
2. Install dependencies:
   ```sh
   npm install
   ```
3. Start the development server:
   ```sh
   npm run dev
   ```

### Docker (Optional)
- Use `docker-compose.yml` to run both backend and frontend in containers:
  ```sh
  docker-compose up --build
  ```

---

## Usage
- Register a new account or log in.
- Set up your profile.
- Search for users and send friend requests.
- Accept or reject incoming friend requests.
- Start chats and send messages in real time.
- Block or unblock users as needed.

---

## License
MIT 