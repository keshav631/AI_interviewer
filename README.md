# 🚀 AI-Interviewer: Autonomous Technical Assessment Platform

> A full-stack AI-powered technical interview simulator that generates dynamic questions, transcribes voice answers, evaluates code submissions, and provides instant AI-driven feedback with performance analytics.

**Status**: Backend MVP + AI Service Foundation | Frontend & Proctoring (In Development)

---

## 🌟 Project Vision

AI-Interviewer aims to democratize technical interview preparation by combining:

- **Dynamic Question Generation**: Real-time questions tailored to role, level, and question type
- **Hybrid Input System**: Voice transcription + code editor for comprehensive evaluation
- **AI-Driven Feedback**: Technical scoring + confidence assessment with detailed explanations
- **Session Analytics**: Per-question breakdown and overall performance metrics
- **Advanced Proctoring** _(Planned)_: Real-time face detection, phone detection, tab-switch monitoring

---

## 📋 Current Implementation Status

### ✅ Implemented (Backend)

- [x] User Authentication (Email/Password + Google OAuth)
- [x] JWT-based Session Management
- [x] Session Lifecycle State Machine (pending → in-progress → completed/failed)
- [x] Interview Session CRUD Operations
- [x] Async Question Generation via AI Service
- [x] Answer Submission (Audio + Code)
- [x] AI Evaluation Pipeline with Score Calculation
- [x] Error Handling Middleware
- [x] Audio File Upload with Validation (10MB, audio-only)
- [x] MongoDB Schema Design (User, Session, Question sub-docs)
- [x] Socket.io Integration for Real-Time Updates

### 🔄 In Progress

- [ ] FastAPI AI Service (Question generation, transcription, evaluation)
- [ ] Frontend (React Vite, Redux, Monaco Editor)
- [ ] Real-time Proctoring (Computer Vision via TensorFlow.js)

### 📅 Planned

- [ ] Advanced Integrity Scoring (Tab switching, face detection, phone detection)
- [ ] Selection Probability Reports
- [ ] Communication Index Analytics
- [ ] Performance Visualization (Chart.js)
- [ ] Deployment (Docker, AWS/Vercel)

---

## 🛠️ Tech Stack

| Layer              | Technology                                                                       |
| ------------------ | -------------------------------------------------------------------------------- |
| **Frontend**       | React 18, Redux Toolkit, Tailwind CSS, Monaco Editor, Socket.io-client (Planned) |
| **Backend**        | Node.js, Express.js, MongoDB (Mongoose), JWT, Multer                             |
| **AI Service**     | Python, FastAPI, Ollama (Mistral), OpenAI Whisper                                |
| **Real-Time**      | Socket.io                                                                        |
| **Authentication** | bcryptjs, JSON Web Tokens, Google OAuth 2.0                                      |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                   Frontend (React Vite)                      │
│  (Planned: Monaco Editor, Voice I/O, Proctoring UI)         │
└────────────────────────┬────────────────────────────────────┘
                         │ Socket.io / REST API
                         ▼
┌─────────────────────────────────────────────────────────────┐
│              Backend (Node.js / Express)                     │
│  ├─ Auth Controller (Register, Login, Profile)             │
│  ├─ Session Controller (Create, Get, Submit, Evaluate)     │
│  ├─ Error Middleware                                        │
│  ├─ Upload Middleware (Audio validation)                   │
│  └─ Database Layer (MongoDB + Mongoose)                    │
└────────────────────────┬────────────────────────────────────┘
                         │ HTTP Requests
                         ▼
┌─────────────────────────────────────────────────────────────┐
│            AI Service (Python / FastAPI)                     │
│  ├─ Question Generation (Ollama Mistral)                    │
│  ├─ Audio Transcription (Whisper)                           │
│  ├─ Answer Evaluation (AI Logic)                            │
│  └─ Scoring Engine                                          │
└─────────────────────────────────────────────────────────────┘
```

---

## 📊 Data Models

### User

```js
{
  name: String,
  email: String (unique),
  password: String (bcrypt hashed),
  googleId: String (optional),
  preferredRole: String (default: "MERN Stack Developer"),
  createdAt: Date,
  updatedAt: Date
}
```

### Session

```js
{
  user: ObjectId (ref: User),
  role: String (e.g., "MERN", "Python", "Data Science"),
  level: String (enum: "junior", "mid", "senior"),
  interviewType: String (enum: "oral-only", "coding-mix"),
  status: String (enum: "pending", "in-progress", "completed", "failed"),
  questions: [Question],
  overallScore: Number (0-100),
  metrics: {
    avgTechnical: Number,
    avgConfidence: Number
  },
  startTime: Date,
  endTime: Date,
  createdAt: Date,
  updatedAt: Date
}
```

### Question (Sub-Document)

```js
{
  questionText: String,
  questionType: String (enum: "coding", "oral"),
  idealAnswer: String,
  userAnswerText: String,
  userSubmittedCode: String,
  isSubmitted: Boolean,
  isEvaluated: Boolean,
  technicalScore: Number (0-100),
  confidenceScore: Number (0-100),
  aiFeedback: String
}
```

---

## 🔌 API Endpoints

### Authentication

- `POST /api/users/register` - Register new user
- `POST /api/users/login` - Login with email/password
- `POST /api/users/google` - Google OAuth login
- `GET /api/users/profile` - Get user profile (Protected)
- `PUT /api/users/profile` - Update user profile (Protected)

### Sessions

- `GET /api/sessions` - List all user sessions (Protected)
- `POST /api/sessions` - Create new session, trigger async question generation (Protected)
- `GET /api/sessions/:id` - Get session details (Protected)
- `DELETE /api/sessions/:id` - Delete session (Protected)
- `POST /api/sessions/:id/submit-answer` - Submit answer (audio/code) (Protected)
- `POST /api/sessions/:id/end` - End session early (Protected)

### AI Service (FastAPI)

- `POST /generate-questions` - Generate interview questions
- `POST /transcribe` - Transcribe audio file to text
- `POST /evaluate` - Evaluate answer and return scores

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** v16+
- **Python** 3.9+
- **MongoDB** (local or Atlas)
- **Ollama** (installed & running with `mistral` model)
- **FFmpeg** (in system PATH for audio processing)

### Installation

#### 1. Clone Repository

```bash
git clone https://github.com/keshav631/AI_interviewer.git
cd AI_interviewer
```

#### 2. Backend Setup (Node.js)

```bash
cd backend
npm install
cp .env.example .env  # Configure your .env
npm run dev
```

**Backend runs on**: `http://localhost:5000`

#### 3. AI Service Setup (Python)

```bash
cd ../AI_services
pip install -r requirements.txt
python main.py
```

**AI Service runs on**: `http://localhost:8000`

#### 4. Frontend Setup (React) _(Planned)_

```bash
cd ../frontend
npm install
npm run dev
```

**Frontend runs on**: `http://localhost:5173`

---

## 🔑 Environment Variables

### Backend `.env`

```env
MONGO_URI=mongodb://localhost:27017/ai-interviewer
JWT_SECRET=your_jwt_secret_key
GOOGLE_CLIENT_ID=your_google_client_id
PORT=5000
NODE_ENV=development
AI_SERVICE_URL=http://localhost:8000
```

### AI Service `.env` (if needed)

```env
OLLAMA_URL=http://localhost:11434
WHISPER_MODEL=base.en
```

---

## 🧠 Core Features Breakdown

### 1. Dynamic Question Generation

- AI generates role-specific questions based on:
  - **Role**: MERN, Python, Data Science, etc.
  - **Level**: Junior, Mid, Senior
  - **Type**: Oral or Coding
- Questions are stored in MongoDB and delivered in real-time

### 2. Answer Submission Flow

1. Client records audio OR writes code (or both)
2. Audio file uploaded via Multer middleware (validated)
3. Backend stores submission status
4. **Async evaluation** triggered:
   - Audio transcribed to text (Whisper)
   - Both transcription + code sent to AI service
   - Scores + feedback computed
   - Socket.io notification sent to client

### 3. Performance Metrics

- **Technical Score**: Evaluates correctness and efficiency
- **Confidence Score**: Analyzes clarity and articulation
- **Overall Score**: Average of technical + confidence
- **Per-Question Breakdown**: Individual feedback and scores

---

## 🔐 Security Features

✅ Password hashing with `bcryptjs` (pre-save hook)  
✅ JWT authentication (1-day expiry)  
✅ Google OAuth 2.0 integration  
✅ Ownership verification (users access only their sessions)  
✅ File upload validation (10MB, audio-only)  
✅ Error middleware (consistent error response format)  
✅ Protected routes for session/profile endpoints

---

## 📈 Performance & Scalability

- **Async Question Generation**: Non-blocking, Socket.io updates keep client informed
- **Session State Machine**: Prevents race conditions and invalid state transitions
- **Aggregation Pipeline**: MongoDB `$group` + `$avg` for efficient score calculation
- **Multer Integration**: Secure file streaming without memory bloat

---

## 🎯 Resume Highlights

1. **Full-Stack Architecture**: Multi-service design (Node.js + Python)
2. **Real-Time Communication**: Socket.io for live session updates
3. **Async Processing**: Background task handling without blocking user requests
4. **Security Best Practices**: JWT, bcrypt, ownership checks, input validation
5. **Database Design**: Nested schemas, proper indexing, aggregation pipelines
6. **Error Handling**: Centralized middleware, consistent API responses
7. **Scalability Mindset**: Designed for horizontal scaling with microservices

---

## 📝 Example Workflow

### 1. Register & Setup

```bash
POST /api/users/register
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "secure123"
}
```

### 2. Create Interview Session

```bash
POST /api/sessions
{
  "role": "MERN Stack Developer",
  "level": "mid",
  "interviewType": "coding-mix",
  "count": 5
}
```

→ Returns `202 Accepted`, questions generated in background  
→ Socket.io event: `sessionUpdate` with status "QUESTIONS_READY"

### 3. Submit Answer

```bash
POST /api/sessions/:id/submit-answer
Content-Type: multipart/form-data
{
  "questionIndex": 0,
  "audioFile": [binary audio data],
  "code": "console.log('Hello World')"
}
```

→ Returns `202 Accepted`, evaluation happens asynchronously  
→ Socket.io event: `sessionUpdate` with scores and feedback

### 4. View Results

```bash
GET /api/sessions/:id
```

→ Returns session with all questions, scores, and feedback

---

## 🚧 Known Limitations & Future Improvements

- **Proctoring**: Real-time face/phone detection not yet implemented
- **Advanced Analytics**: Communication Index, Selection Probability reports planned
- **Deployment**: Docker containerization, cloud hosting setup pending
- **Testing**: Comprehensive unit/integration tests to be added
- **Rate Limiting**: API rate limiting for production readiness
- **Caching**: Redis for session state and frequently accessed data

---

## 📚 Learning Resources & References

- [Express.js Best Practices](https://expressjs.com)
- [MongoDB Aggregation](https://docs.mongodb.com/manual/reference/operator/aggregation)
- [Socket.io Documentation](https://socket.io/docs)
- [FastAPI Official Guide](https://fastapi.tiangolo.com)
- [OpenAI Whisper](https://github.com/openai/whisper)
- [Ollama Documentation](https://ollama.ai)

---

## 👨‍💻 Author

**Keshav** - Full Stack Developer & AI Enthusiast

---

## ⚖️ License

This project is licensed under the **MIT License** - see the LICENSE file for details.

---

## 🤝 Contributing

This is a personal portfolio project. For feedback, suggestions, or collaboration inquiries, feel free to reach out!

---

**Last Updated**: April 13, 2026  
**Current Phase**: Backend MVP + AI Service Foundation
