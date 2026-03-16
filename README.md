# Unified Assessment Management Platform (UAMP)

A **full-stack Unified Assessment Management System** designed for higher education institutions to create, deliver, monitor, and evaluate online assessments with advanced **AI-powered grading, proctoring, and academic integrity analysis**.

The platform integrates **exam management, behavioral analytics, AI evaluation, and fraud detection** into a single scalable architecture.

---

# Overview

The Unified Assessment Management Platform provides an **end-to-end digital assessment lifecycle**, enabling institutions to conduct secure and intelligent online examinations.

### Key capabilities

* Online assessment creation and delivery
* AI-powered answer evaluation
* Real-time exam monitoring and anomaly detection
* Behavioral biometric analysis
* Academic integrity and fraud detection
* Advanced analytics and reporting dashboards
* Real-time notifications and collaboration

The system supports multiple roles including **students, instructors, and administrators**, each with specialized capabilities.

---

# Architecture

The platform follows a **modern full-stack architecture**.

```
React SPA (localhost:3000)
        ↓ HTTP / WebSocket
Axios + Socket.IO
        ↓
Express.js REST API (localhost:5000)
        ↓
Mongoose ODM
        ↓
MongoDB (localhost:27017/uap_db)
```

Frontend communicates with the backend using **REST APIs and WebSockets**, while MongoDB serves as the primary database for persistent storage.

---

# Technology Stack

| Layer                   | Technology                        |
| ----------------------- | --------------------------------- |
| Frontend                | React 18, Vite                    |
| UI Framework            | Ant Design, TailwindCSS           |
| Routing                 | React Router v6                   |
| Backend                 | Node.js, Express.js               |
| Database                | MongoDB with Mongoose ODM         |
| Real-Time Communication | Socket.IO                         |
| AI Evaluation           | Google Gemini AI, OpenAI fallback |
| Authentication          | JWT (90-day tokens), bcrypt       |
| Code Editor             | Monaco Editor                     |
| Data Visualization      | Recharts, React Force Graph 2D    |
| Testing                 | Jest, Supertest, Playwright       |

---

# User Roles

### Student

* Take scheduled online assessments
* Submit answers and coding solutions
* View results and analytics
* Access personal dashboard

### Instructor

* Create and manage assessments
* Evaluate student submissions
* Generate questions using AI
* Monitor exam integrity and analytics

### Administrator

* Manage platform users
* Monitor system activity and anomalies
* Configure platform settings
* Investigate academic integrity violations

---

# Core System Modules

## Authentication and User Management

Secure authentication using **JWT-based authorization** with role-based access control.

Features include:

* User registration and login
* Password encryption using bcrypt
* Role-based access permissions
* Account activation and suspension
* Per-user AI API key storage

### Typing Baseline Biometric

The system records typing behavior to build a **behavioral biometric profile**, including:

* Typing speed
* Keystroke timing
* Typing rhythm

This helps verify identity during assessments.

---

# Assessment Engine

The assessment engine enables instructors to design and manage exams.

### Supported Question Types

* Multiple Choice (MCQ)
* Multiple Answer
* Short Answer
* Long Answer / Essay
* Programming / Coding

### Assessment Configuration

* Start and expiration scheduling
* Student enrollment
* Question randomization
* Attempt limits
* Passing score configuration

### Adaptive Difficulty

The platform dynamically adjusts question difficulty based on student performance.

```
Correct answer → harder question
Incorrect answer → easier question
```

This creates personalized learning assessments.

---

# Student Exam Interface

Students take assessments through a **dedicated proctored exam interface**.

Key features include:

* Question navigation
* Per-question timer tracking
* Real-time telemetry monitoring
* Coding environment with Monaco Editor

Coding questions support:

* syntax highlighting
* execution environments
* automated test case evaluation

---

# Proctoring and Anomaly Detection

The system monitors student behavior during exams to detect suspicious activity.

### Telemetry Signals Tracked

* Tab switching
* Copy and paste events
* Window focus loss
* IP address changes
* Keystroke dynamics
* Rapid answering patterns

### Trust Score Calculation

```
Trust Score =
100
- (TabSwitch × 5)
- (CopyPaste × 10)
- (IPChange × 20)
- (FastAnswer × 3)
```

Risk levels:

| Score | Risk Level |
| ----- | ---------- |
| 80+   | Low        |
| 50–80 | Medium     |
| <50   | High       |

### Exam DNA

Each exam session generates a unique **SHA-256 behavioral fingerprint** based on telemetry patterns.

---

# AI-Powered Evaluation

The platform uses **Google Gemini AI** to automatically evaluate:

* short answers
* long form essays
* programming solutions

### Multi-Model Fallback

The system automatically retries evaluation across multiple AI models if one fails.

```
Gemini Model A
→ Gemini Model B
→ Gemini Model C
→ OpenAI fallback
```

This ensures reliability and consistent evaluation.

---

# Academic Integrity and Fraud Detection

The system includes advanced mechanisms for detecting cheating and collusion.

### Collusion Detection

Student answers are compared using **Jaccard similarity analysis**.

```
Similarity = Intersection / Union
```

If similarity exceeds **85%**, the system flags potential collusion.

---

### Pattern Analysis

Worker threads analyze patterns such as:

* rapid submission clusters
* impossible geographic travel
* synchronized answer patterns

These analyses run asynchronously to avoid blocking the main server.

---

# Blockchain-Style Audit Logs

All system activity logs are stored in a **tamper-evident hash chain**.

Each log entry includes:

* previousHash
* currentHash
* timestamp
* action metadata

This ensures audit trails cannot be altered without detection.

### Merkle Tree Verification

Batch audit verification uses **Merkle trees** to validate log integrity efficiently.

---

# Analytics and Reporting

The platform provides detailed analytics dashboards.

### Instructor Analytics

* submission statistics
* question difficulty metrics
* average time per question
* student performance distribution

### Administrator Analytics

* platform usage metrics
* anomaly monitoring
* academic integrity reports

### Collusion Network Visualization

Suspicious student relationships are visualized using a **network graph** powered by React Force Graph.

---

# Notifications

Real-time notifications are delivered through **Socket.IO**.

Examples include:

* new assessment availability
* submission confirmations
* high-risk anomaly alerts
* system announcements

---

# Database Models

| Model                  | Purpose                                |
| ---------------------- | -------------------------------------- |
| User                   | Authentication, roles, typing baseline |
| Assessment             | Questions, settings, schedules         |
| Submission             | Student answers and scores             |
| AuditLog               | Tamper-evident audit records           |
| ExamSession            | Telemetry session data                 |
| Notification           | User notification queue                |
| ActivityLog            | System activity logs                   |
| PlatformSettings       | Global configuration                   |
| Category / Subcategory | Assessment organization                |

---

# API Endpoints

| Group         | Base Path          | Key Operations           |
| ------------- | ------------------ | ------------------------ |
| Auth          | /api/auth          | login, register, profile |
| Assessments   | /api/assessments   | CRUD, enroll             |
| Submissions   | /api/submissions   | submit, evaluate         |
| Admin         | /api/admin         | stats, logs              |
| AI            | /api/ai            | generate, evaluate       |
| Telemetry     | /api/telemetry     | exam event logging       |
| Notifications | /api/notifications | fetch, mark read         |
| Analytics     | /api/analytics     | collusion graph          |

---

# Running the Project

## 1. Start MongoDB

```
docker run -d --name mongodb -p 27017:27017 mongo
```

---

## 2. Start Backend

```
cd backend
npm install
```

Create `.env` file:

```
PORT=5000
MONGO_URI=your_mongodb_uri
JWT_SECRET=your_secret
GEMINI_API_KEY=your_key
CLIENT_URL=http://localhost:3000
```

Start server:

```
npm run dev
```

---

## 3. Start Frontend

```
cd frontend
npm install
```

Create `.env` file:

```
VITE_API_URL=http://127.0.0.1:5000/api
```

Run development server:

```
npm run dev
```

---

# Access Application

```
Frontend
http://localhost:3000

Backend API
http://localhost:5000
```

---

# Architecture Highlights

* Worker threads for heavy fraud detection workloads
* Blockchain-style audit logs with hash chaining
* Merkle tree verification for audit batches
* Adaptive difficulty assessment engine
* Multi-model AI fallback architecture
* Explainable AI (XAI) anomaly explanations

---

# Project Scope

This system contains **over 120 source files** implementing a full lifecycle assessment platform, covering:

* exam creation
* delivery
* proctoring
* AI evaluation
* fraud detection
* analytics
* reporting

The platform is designed as a **scalable academic assessment infrastructure for modern educational institutions**.
