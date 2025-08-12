### Base
- **Base URL**: `http://localhost:8080`
- **Cookie jar (for auth)**: Use `-c .cookies` to save cookies and `-b .cookies` to send them.
- Most authenticated endpoints rely on the `accessToken` cookie. Refresh at `/api/v1/auth/refresh` when needed.

### Auth
- **POST** `/api/v1/auth/register`
  - Body (JSON):
```json
{
  "username": "youruser01",
  "email": "youruser01@example.com",
  "password": "yourpassword",
  "confirmPassword": "yourpassword"
}
```
  - Curl (PowerShell):
```bash
curl -X POST "http://localhost:8080/api/v1/auth/register" `
  -H "Content-Type: application/json" `
  -d '{"username":"youruser01","email":"youruser01@example.com","password":"yourpassword","confirmPassword":"yourpassword"}' `
  -c .cookies
```

- **POST** `/api/v1/auth/login`
  - Body (JSON):
```json
{
  "identifier": "youruser01", // username or email
  "password": "yourpassword"
}
```
  - Curl:
```bash
curl -X POST "http://localhost:8080/api/v1/auth/login" `
  -H "Content-Type: application/json" `
  -d '{"identifier":"youruser01","password":"yourpassword"}' `
  -c .cookies
```

- **POST** `/api/v1/auth/refresh`
  - Uses `refreshToken` cookie; returns new `accessToken` cookie.
```bash
curl -X POST "http://localhost:8080/api/v1/auth/refresh" -b .cookies -c .cookies
```

- **POST** `/api/v1/auth/logout`
  - Clears auth cookies.
```bash
curl -X POST "http://localhost:8080/api/v1/auth/logout" -b .cookies -c .cookies
```

### Users
- **POST** `/api/v1/users/setup-basic-info` (multipart)
  - Parts:
    - `userInfoRequest` (application/json): `{ "fullName": "John Doe", "bio": "Hello!" }`
    - `profileImageFile` (optional file)
  - Curl:
```bash
curl -X POST "http://localhost:8080/api/v1/users/setup-basic-info" `
  -b .cookies -c .cookies `
  -F "userInfoRequest={\"fullName\":\"John Doe\",\"bio\":\"Hello!\"};type=application/json" `
  -F "profileImageFile=@C:/path/to/image.png;type=image/png"
```

- **GET** `/api/v1/users/me`
```bash
curl "http://localhost:8080/api/v1/users/me" -b .cookies
```

### Chats
- **POST** `/api/v1/chats` (multipart)
  - Parts:
    - `createChatRequest` (application/json):
```json
{
  "name": "My Group",
  "participants": ["<userId1>", "<userId2>"],
  "admins": ["<userId1>"]
}
```
  - Curl:
```bash
curl -X POST "http://localhost:8080/api/v1/chats" `
  -b .cookies -c .cookies `
  -F "createChatRequest={\"name\":\"My Group\",\"participants\":[\"<userId1>\",\"<userId2>\"],\"admins\":[\"<userId1>\"]};type=application/json" `
  -F "chatImageFile=@C:/path/to/chat.png;type=image/png"
```

- **GET** `/api/v1/chats/{chatId}`
```bash
curl "http://localhost:8080/api/v1/chats/<chatId>" -b .cookies
```

- **PUT** `/api/v1/chats/{chatId}` (multipart)
  - Parts:
    - `updateChatRequest` (application/json): `{ "name": "New Name" }`
    - `chatImageFile` optional file
```bash
curl -X PUT "http://localhost:8080/api/v1/chats/<chatId>" `
  -b .cookies -c .cookies `
  -F "updateChatRequest={\"name\":\"New Name\"};type=application/json" `
  -F "chatImageFile=@C:/path/to/new.png;type=image/png"
```

- **DELETE** `/api/v1/chats/{chatId}`
```bash
curl -X DELETE "http://localhost:8080/api/v1/chats/<chatId>" -b .cookies
```

- **GET** `/api/v1/chats/my-chats`
  - Query: `pageNumber` (1), `pageSize` (20), `sortBy` (updatedAt), `sortDirection` (desc), `beforeChatId` (optional)
```bash
curl "http://localhost:8080/api/v1/chats/my-chats?pageNumber=1&pageSize=20&sortBy=updatedAt&sortDirection=desc" -b .cookies
```

- **GET** `/api/v1/chats/{chatId}/messages`
  - Query: `pageNumber` (1), `pageSize` (20), `sortBy` (createdAt), `sortDirection` (desc), `beforeMessageId` (optional)
```bash
curl "http://localhost:8080/api/v1/chats/<chatId>/messages?pageNumber=1&pageSize=20&sortBy=createdAt&sortDirection=desc" -b .cookies
```

- **PUT** `/api/v1/chats/mark-as-read/{chatId}`
```bash
curl -X PUT "http://localhost:8080/api/v1/chats/mark-as-read/<chatId>" -b .cookies
```

### Messages
- **POST** `/api/v1/messages/send` (multipart)
  - Parts:
    - `sendMessageRequest` (application/json):
```json
{ "chatId": "<chatId>", "senderId": "<userId>", "content": "Hello" }
```
    - `mediaFile` optional file
  - Curl:
```bash
curl -X POST "http://localhost:8080/api/v1/messages/send" `
  -b .cookies -c .cookies `
  -F "sendMessageRequest={\"chatId\":\"<chatId>\",\"senderId\":\"<userId>\",\"content\":\"Hello\"};type=application/json" `
  -F "mediaFile=@C:/path/to/file.jpg;type=image/jpeg"
```

### User Graph (Neo4j)
- Paging defaults: `pageNumber=1`, `pageSize=10` unless noted

- **GET** `/api/v1/user-nodes/all`
```bash
curl "http://localhost:8080/api/v1/user-nodes/all?pageNumber=1&pageSize=10" -b .cookies
```

- **GET** `/api/v1/user-nodes/friends`
```bash
curl "http://localhost:8080/api/v1/user-nodes/friends?pageNumber=1&pageSize=10" -b .cookies
```

- **GET** `/api/v1/user-nodes/blocked`
```bash
curl "http://localhost:8080/api/v1/user-nodes/blocked?pageNumber=1&pageSize=10" -b .cookies
```

- **GET** `/api/v1/user-nodes/friend-requests/incoming`
```bash
curl "http://localhost:8080/api/v1/user-nodes/friend-requests/incoming?pageNumber=1&pageSize=10" -b .cookies
```

- **GET** `/api/v1/user-nodes/friend-requests/sent`
```bash
curl "http://localhost:8080/api/v1/user-nodes/friend-requests/sent?pageNumber=1&pageSize=10" -b .cookies
```

- **POST** `/api/v1/user-nodes/friend-request/send/{targetUserId}`
```bash
curl -X POST "http://localhost:8080/api/v1/user-nodes/friend-request/send/<targetUserId>" -b .cookies
```

- **POST** `/api/v1/user-nodes/friend-request/cancel/{targetUserId}`
```bash
curl -X POST "http://localhost:8080/api/v1/user-nodes/friend-request/cancel/<targetUserId>" -b .cookies
```

- **POST** `/api/v1/user-nodes/friend-request/accept/{targetUserId}`
```bash
curl -X POST "http://localhost:8080/api/v1/user-nodes/friend-request/accept/<targetUserId>" -b .cookies
```

- **POST** `/api/v1/user-nodes/friend-request/reject/{targetUserId}`
```bash
curl -X POST "http://localhost:8080/api/v1/user-nodes/friend-request/reject/<targetUserId>" -b .cookies
```

- **DELETE** `/api/v1/user-nodes/friends/{targetUserId}`
```bash
curl -X DELETE "http://localhost:8080/api/v1/user-nodes/friends/<targetUserId>" -b .cookies
```

- **POST** `/api/v1/user-nodes/block/{targetUserId}`
```bash
curl -X POST "http://localhost:8080/api/v1/user-nodes/block/<targetUserId>" -b .cookies
```

- **POST** `/api/v1/user-nodes/unblock/{targetUserId}`
```bash
curl -X POST "http://localhost:8080/api/v1/user-nodes/unblock/<targetUserId>" -b .cookies
```

- **GET** `/api/v1/user-nodes/search-friends`
  - Query: `query`, `pageNumber`, `pageSize`, `sortBy` (fullName), `sortDirection` (asc)
```bash
curl "http://localhost:8080/api/v1/user-nodes/search-friends?query=jo&pageNumber=1&pageSize=20&sortBy=fullName&sortDirection=asc" -b .cookies
```

- **GET** `/api/v1/user-nodes/search`
  - Query: `query`, `pageNumber`, `pageSize`, `sortBy` (fullName), `sortDirection` (asc)
```bash
curl "http://localhost:8080/api/v1/user-nodes/search?query=jo&pageNumber=1&pageSize=10&sortBy=fullName&sortDirection=asc" -b .cookies
```

### Stream
- **GET** `/api/v1/stream/token`
```bash
curl "http://localhost:8080/api/v1/stream/token" -b .cookies
```

### WebSocket (FYI)
- STOMP endpoint: `ws://localhost:8080/ws` (SockJS enabled)
- App prefix: `/app`, broker: `/topic`, `/queue`
- Typing event: send to `/app/chat/{chatId}/typing` with JSON
```json
{ "type": "START", "userId": "<userId>", "chatId": "<chatId>" }
```

### Notes
- Security currently permits all (`/**`), but `/api/v1/users/me` requires a valid `accessToken` cookie.
- Multipart JSON parts must include `;type=application/json`.
- Use `-b .cookies -c .cookies` on curl to persist session cookies across calls. 