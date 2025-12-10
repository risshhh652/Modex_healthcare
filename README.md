# Modex_healthcare
# 🧠 Mental Health Support API

A comprehensive MERN stack application providing AI-powered mental health support with mood tracking, activity logging, and therapeutic chat sessions.

[![Live Demo](https://img.shields.io/badge/demo-live-green)](https://your-app.onrender.com)
[![API Status](https://img.shields.io/badge/API-online-success)](https://your-api.onrender.com/health)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

## 🌐 Live Application

## 📋 Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Setup Instructions](#setup-instructions)
- [API Documentation](#api-documentation)
- [Environment Variables](#environment-variables)
- [Deployment](#deployment)
- [Testing](#testing)
- [Contributing](#contributing)

## ✨ Features

### Core Functionality
- 🔐 **User Authentication**: Secure JWT-based authentication with session management
- 💬 **AI-Powered Chat**: Therapeutic conversations using Claude AI
- 📊 **Mood Tracking**: Log and monitor emotional wellbeing (0-100 scale)
- 🏃 **Activity Logging**: Track wellness activities (meditation, exercise, journaling, etc.)
- 📱 **Real-time Sessions**: Manage multiple chat sessions with conversation history
- 🔔 **Background Jobs**: Automated tasks using Inngest

### Security Features
- Password hashing with bcrypt
- JWT token authentication
- Session expiry management
- Protected API routes
- CORS enabled

## 🛠️ Tech Stack

### Backend
- **Runtime**: Node.js with TypeScript
- **Framework**: Express.js
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: JWT (jsonwebtoken) + bcrypt
- **AI Integration**: Anthropic Claude API
- **Background Jobs**: Inngest
- **Security**: Helmet, CORS
- **Logging**: Winston/Morgan

### Frontend (If applicable)
- **Framework**: React.js
- **State Management**: Context API / Redux
- **HTTP Client**: Axios
- **Routing**: React Router
- **UI Components**: Material-UI / Tailwind CSS

## 🏗️ Architecture

```
┌─────────────────┐
│   React Client  │
└────────┬────────┘
         │
         ↓ HTTPS
┌─────────────────┐
│  Express API    │
│  - Auth Routes  │
│  - Chat Routes  │
│  - Mood Routes  │
│  - Activity     │
└────────┬────────┘
         │
    ┌────┴────┬─────────────┬──────────────┐
    ↓         ↓             ↓              ↓
┌─────────┐ ┌──────────┐ ┌─────────┐ ┌─────────┐
│ MongoDB │ │ Claude   │ │ Inngest │ │ Winston │
│         │ │ AI API   │ │ Jobs    │ │ Logger  │
└─────────┘ └──────────┘ └─────────┘ └─────────┘
```

## 🚀 Setup Instructions

### Prerequisites
- Node.js (v16 or higher)
- MongoDB (v4.4 or higher)
- npm or yarn
- Git

### Local Development Setup

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/mental-health-api.git
cd mental-health-api
```

2. **Install dependencies**
```bash
# Backend
npm install

# Frontend (if separate)
cd client
npm install
```

3. **Environment Setup**

Create a `.env` file in the root directory:
```env
# Server
PORT=3001
NODE_ENV=development

# Database
MONGODB_URI=mongodb://localhost:27017/mental-health-db

# Authentication
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
JWT_EXPIRES_IN=7d

# Anthropic AI
ANTHROPIC_API_KEY=your-anthropic-api-key

# Inngest
INNGEST_EVENT_KEY=your-inngest-event-key
INNGEST_SIGNING_KEY=your-inngest-signing-key

# CORS
CORS_ORIGIN=http://localhost:3001
```

4. **Start MongoDB**
```bash
# Using MongoDB service
sudo systemctl start mongod

# Or using Docker
docker run -d -p 27017:27017 --name mongodb mongo:latest
```

5. **Run the application**
```bash
# Development mode with hot reload
npm run dev

# Production mode
npm run build
npm start
```

6. **Access the application**
- API: http://localhost:3001
- Health Check: http://localhost:3001/health
- Frontend: http://localhost:3000 (if applicable)

## 📚 API Documentation

### Base URL
```
Production: https://your-api.onrender.com
Development: http://localhost:3001
```

### Authentication
All protected endpoints require a Bearer token in the Authorization header:
```
Authorization: Bearer <your-jwt-token>
```

### Postman Collection
[![Run in Postman](https://run.pstmn.io/button.svg)](https://documenter.getpostman.com/view/your-collection-id)

Import the collection: [Download Postman Collection](./postman_collection.json)

### API Endpoints

#### 🔐 Authentication

**Register User**
```http
POST /auth/register
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "SecurePass123!"
}

Response: 201 Created
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "6584d5f2e8c9a1234567890",
    "name": "John Doe",
    "email": "john@example.com"
  }
}
```

**Login**
```http
POST /auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "SecurePass123!"
}

Response: 200 OK
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "6584d5f2e8c9a1234567890",
    "name": "John Doe",
    "email": "john@example.com"
  }
}
```

**Get Current User**
```http
GET /auth/me
Authorization: Bearer <token>

Response: 200 OK
{
  "user": {
    "id": "6584d5f2e8c9a1234567890",
    "name": "John Doe",
    "email": "john@example.com"
  }
}
```

**Logout**
```http
POST /auth/logout
Authorization: Bearer <token>

Response: 200 OK
{
  "message": "Logged out successfully"
}
```

#### 💬 Chat Sessions

**Create Chat Session**
```http
POST /chat/sessions
Authorization: Bearer <token>
Content-Type: application/json

{
  "initialMessage": "I'm feeling anxious today"
}

Response: 201 Created
{
  "sessionId": "sess_1234567890",
  "message": "Session created successfully"
}
```

**Get Chat Session**
```http
GET /chat/sessions/:sessionId
Authorization: Bearer <token>

Response: 200 OK
{
  "sessionId": "sess_1234567890",
  "userId": "6584d5f2e8c9a1234567890",
  "startTime": "2024-12-10T10:30:00.000Z",
  "status": "active",
  "messages": [...]
}
```

**Send Message**
```http
POST /chat/sessions/:sessionId/messages
Authorization: Bearer <token>
Content-Type: application/json

{
  "message": "What breathing exercises can help with anxiety?"
}

Response: 200 OK
{
  "userMessage": {...},
  "assistantMessage": {
    "role": "assistant",
    "content": "Here are some effective breathing exercises...",
    "timestamp": "2024-12-10T10:35:00.000Z"
  }
}
```

**Get Chat History**
```http
GET /chat/sessions/:sessionId/history
Authorization: Bearer <token>

Response: 200 OK
{
  "sessionId": "sess_1234567890",
  "messages": [
    {
      "role": "user",
      "content": "I'm feeling anxious",
      "timestamp": "2024-12-10T10:30:00.000Z"
    },
    {
      "role": "assistant",
      "content": "I understand...",
      "timestamp": "2024-12-10T10:30:15.000Z"
    }
  ]
}
```

#### 📊 Mood Tracking

**Log Mood**
```http
POST /api/mood
Authorization: Bearer <token>
Content-Type: application/json

{
  "score": 75,
  "note": "Feeling good after meditation"
}

Response: 201 Created
{
  "mood": {
    "id": "6584d5f2e8c9a1234567891",
    "userId": "6584d5f2e8c9a1234567890",
    "score": 75,
    "note": "Feeling good after meditation",
    "timestamp": "2024-12-10T10:00:00.000Z"
  }
}
```

#### 🏃 Activity Logging

**Log Activity**
```http
POST /api/activity
Authorization: Bearer <token>
Content-Type: application/json

{
  "type": "meditation",
  "name": "Morning Mindfulness",
  "description": "10-minute breathing meditation",
  "duration": 10
}

Response: 201 Created
{
  "activity": {
    "id": "6584d5f2e8c9a1234567892",
    "userId": "6584d5f2e8c9a1234567890",
    "type": "meditation",
    "name": "Morning Mindfulness",
    "description": "10-minute breathing meditation",
    "duration": 10,
    "timestamp": "2024-12-10T09:00:00.000Z"
  }
}
```

**Activity Types**
- `meditation`
- `exercise`
- `walking`
- `reading`
- `journaling`
- `therapy`

### Error Responses

```json
{
  "error": "Error message",
  "details": "Additional error details"
}
```

**Common Status Codes**
- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `500` - Internal Server Error

## 🔧 Environment Variables

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `PORT` | Server port | No | 3001 |
| `NODE_ENV` | Environment | No | development |
| `MONGODB_URI` | MongoDB connection string | Yes | - |
| `JWT_SECRET` | JWT signing secret | Yes | - |
| `JWT_EXPIRES_IN` | JWT expiration time | No | 7d |
| `ANTHROPIC_API_KEY` | Claude AI API key | Yes | - |
| `INNGEST_EVENT_KEY` | Inngest event key | Yes | - |
| `INNGEST_SIGNING_KEY` | Inngest signing key | Yes | - |
| `CORS_ORIGIN` | Allowed CORS origins | No | * |

## 🚢 Deployment

### Deploy to Render

1. **Create a new Web Service**
   - Connect your GitHub repository
   - Select "Node" environment
   - Build Command: `npm install && npm run build`
   - Start Command: `npm start`

2. **Add Environment Variables**
   - Go to "Environment" tab
   - Add all required variables from `.env`

3. **Deploy**
   - Click "Create Web Service"
   - Wait for deployment to complete

### Deploy to Heroku

```bash
# Install Heroku CLI
npm install -g heroku

# Login to Heroku
heroku login

# Create app
heroku create your-app-name

# Add MongoDB addon
heroku addons:create mongodb:sandbox

# Set environment variables
heroku config:set JWT_SECRET=your-secret
heroku config:set ANTHROPIC_API_KEY=your-key

# Deploy
git push heroku main

# Open app
heroku open
```

### Deploy to Railway

1. Click "New Project" → "Deploy from GitHub"
2. Select your repository
3. Add environment variables
4. Deploy automatically

### Deploy Frontend (Vercel/Netlify)

**Vercel**
```bash
npm install -g vercel
cd client
vercel --prod
```

**Netlify**
```bash
npm install -g netlify-cli
cd client
npm run build
netlify deploy --prod --dir=build
```

## 🧪 Testing

### Run Tests
```bash
# Unit tests
npm test

# Integration tests
npm run test:integration

# E2E tests
npm run test:e2e

# Coverage
npm run test:coverage
```

### Manual Testing with Postman

1. Import the Postman collection
2. Set environment variables:
   - `baseUrl`: Your API URL
   - `token`: Will auto-populate after login
3. Run the "Auth → Register" or "Auth → Login" request
4. Test other endpoints

## 📖 Technical Documentation

For detailed technical documentation, see [TECHNICAL_DOCUMENTATION.md](./TECHNICAL_DOCUMENTATION.md)

Topics covered:
- System Architecture
- Database Schema
- Authentication Flow
- AI Integration
- Background Jobs
- Security Considerations
- Performance Optimization
- Scalability Strategies

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👥 Authors

- **Your Name** - [GitHub](https://github.com/risshhh652)

## 🙏 Acknowledgments

- Anthropic for Claude AI API
- MongoDB for database solutions
- Express.js community
- All contributors



---

**Note**: Replace placeholder URLs and values with your actual deployment URLs and configuration.