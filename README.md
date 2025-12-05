# TeamLink - Chat Server Backend

Backend chat server for **TeamLink**, a real-time chat server built with Express.js and Socket.io for a video conference platform. This server provides real-time messaging capabilities with Firebase Authentication integration.

## Features

- 🔐 **Firebase Authentication**: Secure token-based authentication using Firebase Admin SDK
- 💬 **Real-time Messaging**: WebSocket-based communication using Socket.io
- 🏠 **Room-based Chat**: Support for multiple chat rooms
- 🔒 **CORS Support**: Configurable CORS for cross-origin requests
- 📦 **TypeScript**: Fully typed codebase for better development experience
- 🚀 **Production Ready**: Environment-based configuration for development and production

## Prerequisites

- Node.js >= 16.0.0
- npm or yarn
- Firebase project with Admin SDK credentials

## Installation

1. Clone the repository:
```bash
git clone https://github.com/jhonkrave/pi-miniproject-3-backend-chat.git
cd pi-miniproject-3-backend-chat
```

2. Install dependencies:
```bash
npm install
```

3. Set up Firebase credentials:
   - Obtain your Firebase service account key JSON file
   - Place it in `api/config/serviceAccountKey.json` (for local development)
   - Or set the `FIREBASE_SERVICE_ACCOUNT_KEY_PATH` environment variable (for production)

## Configuration

### Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
# Server Configuration
PORT=3001

# CORS Configuration (comma-separated for multiple origins)
CORS_ORIGIN=http://localhost:3000,http://localhost:3001

# Firebase Configuration
FIREBASE_SERVICE_ACCOUNT_KEY_PATH=./api/config/serviceAccountKey.json

# Environment
NODE_ENV=development
```

### Firebase Setup

1. Go to your Firebase Console
2. Navigate to Project Settings > Service Accounts
3. Generate a new private key
4. Save the JSON file as `api/config/serviceAccountKey.json`
5. **Important**: Add `api/config/serviceAccountKey.json` to `.gitignore` to avoid committing credentials

## Usage

### Development

Run the server in development mode with hot reload:
```bash
npm run dev
```

### Production

1. Build the TypeScript code:
```bash
npm run build
```

2. Start the server:
```bash
npm start
```

The server will start on `http://localhost:3001` (or the port specified in your `.env` file).

## API Endpoints

### Health Check
- **GET** `/`** - Returns server status message

## Socket.io Events

### Client → Server

#### `join-room`
Join a chat room.

**Payload:**
```typescript
{
  roomId: string;
  userId: string;
}
```

#### `send-message`
Send a message to a room.

**Payload:**
```typescript
{
  roomId: string;
  message: string;
}
```

### Server → Client

#### `receive-message`
Receive a message in a room.

**Payload:**
```typescript
{
  roomId: string;
  message: string;
  senderId: string;
  senderName: string;
  timestamp: string; // ISO 8601 format
}
```

## Authentication

The server uses Firebase Authentication tokens for socket connections. Clients must provide a valid Firebase ID token when connecting:

```javascript
const socket = io('http://localhost:3001', {
  auth: {
    token: 'your-firebase-id-token'
  }
});
```

Or via headers:
```javascript
const socket = io('http://localhost:3001', {
  extraHeaders: {
    token: 'your-firebase-id-token'
  }
});
```

## Project Structure

```
.
├── api/
│   ├── config/
│   │   └── serviceAccountKey.json    # Firebase credentials (not in git)
│   ├── middleware/
│   │   └── auth.ts                   # Firebase token verification
│   ├── services/
│   │   └── socketService.ts          # Socket.io event handlers
│   └── index.ts                      # Server entry point
├── dist/                             # Compiled JavaScript (generated)
├── node_modules/                     # Dependencies
├── .env                              # Environment variables (not in git)
├── package.json
├── tsconfig.json
└── README.md
```

## Available Scripts

- `npm run dev` - Start development server with ts-node
- `npm run build` - Compile TypeScript to JavaScript
- `npm start` - Start production server (requires build first)
- `npm run lint` - Run ESLint

## Technologies

- **Express.js** - Web framework
- **Socket.io** - Real-time bidirectional communication
- **Firebase Admin SDK** - Authentication and user management
- **TypeScript** - Type-safe JavaScript
- **CORS** - Cross-origin resource sharing
- **dotenv** - Environment variable management

## Security Considerations

- ⚠️ **Never commit** `serviceAccountKey.json` to version control
- ⚠️ Use environment variables for sensitive configuration in production
- ⚠️ Configure CORS origins appropriately for your deployment
- ⚠️ Always use HTTPS in production

## Error Handling

The server includes error handling for:
- Missing or invalid authentication tokens
- Firebase initialization failures
- Socket connection errors

## About TeamLink

TeamLink is a video conference platform developed as part of an Integrative Project (Proyecto Integrador). This chat server is one of the backend services that powers TeamLink's real-time communication features.

## Development Notes

- The server is designed to work alongside a separate user management server
- Default port is 3001 to avoid conflicts with other services
- Socket connections require valid Firebase authentication tokens
- Room-based messaging allows multiple concurrent chat sessions
- Part of the TeamLink platform architecture

## License

This project is part of an academic Integrative Project.

## Contributing

This is an academic project for the Integrative Project course.

