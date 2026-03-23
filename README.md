<div align="center">

# 🔐 Biometric App — Anti-Spoofing Face Recognition

**A full-stack biometric authentication system with real-time liveness detection.**  
Secure. Fast. Spoof-resistant.

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-2.x-black?logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=black)](https://reactjs.org/)
[![MUI](https://img.shields.io/badge/MUI-5-007FFF?logo=mui&logoColor=white)](https://mui.com/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Running the App](#-running-the-app)
- [API Reference](#-api-reference)
- [Anti-Spoofing Details](#-anti-spoofing-details)
- [Project Structure](#-project-structure)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🧠 Overview

**Biometric App** is a secure, full-stack face recognition authentication system built with a **React** frontend and a **Flask** backend. It combines deep learning-based face recognition with real-time **liveness detection** to prevent spoofing attacks — such as someone holding up a photo or video to deceive the system.

Users can register with their face, then log in using facial recognition alone. Every login attempt is first screened by anti-spoofing checks (blink detection and head movement analysis) before face matching is performed.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🎭 **Face Registration** | Register users by capturing and encoding their face via webcam |
| 🔑 **Face Login** | Authenticate users by comparing live face encodings against stored data |
| 👁️ **Blink Detection** | Liveness check using Eye Aspect Ratio (EAR) to detect real blinks |
| 🙆 **Head Movement Detection** | Liveness check that verifies natural head pose variation |
| 🛡️ **Anti-Spoofing** | Multi-factor liveness pipeline rejects photos, videos, and masks |
| 👤 **User Management** | View all registered users and delete accounts |
| 📋 **Profile Management** | Retrieve and update user profile information |
| 🌐 **React UI** | Responsive Material UI frontend with live webcam integration |
| 🔄 **CORS-enabled API** | Flask backend with full CORS support for local development |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Browser (React)                       │
│                                                              │
│  ┌──────────────┐   ┌────────────────┐   ┌───────────────┐  │
│  │   Webcam      │   │  React Router  │   │  Material UI  │  │
│  │  (capture)    │   │  (navigation)  │   │  (components) │  │
│  └──────┬───────┘   └────────────────┘   └───────────────┘  │
│         │  Base64 image via Axios (HTTP)                      │
└─────────┼───────────────────────────────────────────────────┘
          │
          ▼
┌─────────────────────────────────────────────────────────────┐
│                     Flask API (run.py)                        │
│                                                              │
│  POST /register      POST /login         GET /users          │
│  GET  /profile       PUT  /profile       DELETE /delete_user │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐  │
│  │              Anti-Spoofing Pipeline                     │  │
│  │  1. Blink Detection (EAR < 0.25 threshold)             │  │
│  │  2. Head Movement Detection (pose angle analysis)      │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐  │
│  │              Face Recognition Pipeline                  │  │
│  │  face_recognition lib  ·  dlib 68-point landmarks      │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                              │
│                   user_data/users.json                       │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

### Backend
| Technology | Purpose |
|---|---|
| **Python 3.8+** | Core runtime |
| **Flask** | REST API server |
| **Flask-CORS** | Cross-origin resource sharing |
| **face_recognition** | Face encoding & matching (dlib-based) |
| **dlib** | Facial landmark detection (68-point model) |
| **OpenCV (cv2)** | Image processing |
| **NumPy** | Numerical operations |
| **imutils** | Face utility helpers |
| **scipy** | Spatial distance calculations |
| **Pillow (PIL)** | Image decoding |

### Frontend
| Technology | Purpose |
|---|---|
| **React 18** | UI framework |
| **React Router v6** | Client-side routing |
| **Material UI (MUI v5)** | Component library |
| **Axios** | HTTP client |
| **react-webcam** | Webcam capture |

---

## 📦 Prerequisites

Make sure you have the following installed before proceeding:

- **Python 3.8+** — [Download](https://www.python.org/downloads/)
- **Node.js 16+** and **npm** — [Download](https://nodejs.org/)
- **CMake** (required to build dlib) — `sudo apt install cmake` or [cmake.org](https://cmake.org/)
- A working **webcam**

> **Note:** The `shape_predictor_68_face_landmarks.dat` model file is already included in this repository.

---

## 🚀 Installation

### 1. Clone the repository

```bash
git clone https://github.com/RaptorBlingx/biometric-app-new.git
cd biometric-app-new
```

### 2. Set up the Python backend

```bash
# Create and activate a virtual environment
python -m venv venv

# On Windows:
.\venv\Scripts\activate

# On macOS/Linux:
source venv/bin/activate

# Install Python dependencies
pip install flask flask-cors face_recognition dlib opencv-python numpy imutils scipy pillow
```

### 3. Set up the React frontend

```bash
# Navigate to the React app directory
cd anti-spoofing-face-recognition

# Install Node dependencies
npm install
```

---

## ▶️ Running the App

You will need **two terminal windows** running simultaneously.

### Terminal 1 — Flask Backend

```bash
# From the project root, with virtualenv activated
python run.py
```

The Flask server will start at: **http://localhost:5000**

### Terminal 2 — React Frontend

```bash
# From the anti-spoofing-face-recognition directory
npm start
```

The React app will open at: **http://localhost:3000**

---

## 📡 API Reference

All endpoints are served at `http://localhost:5000`.

### `POST /register`

Register a new user with their face.

**Request Body:**
```json
{
  "username": "john_doe",
  "face_data": "data:image/jpeg;base64,..."
}
```

**Responses:**
| Status | Body |
|---|---|
| `200` | `{ "status": "User registered successfully" }` |
| `400` | `{ "status": "User already exists" }` |

---

### `POST /login`

Authenticate a user via face recognition (includes anti-spoofing checks).

**Request Body:**
```json
{
  "face_data": "data:image/jpeg;base64,..."
}
```

**Responses:**
| Status | Body |
|---|---|
| `200` | `{ "status": "Login successful", "username": "john_doe" }` |
| `400` | `{ "status": "Spoofing detected" }` |
| `400` | `{ "status": "Login failed" }` |

---

### `GET /users`

Returns a list of all registered usernames and their stored face encodings.

**Response:**
```json
[
  { "username": "john_doe", "face_encoding": [ ... ] }
]
```

---

### `DELETE /delete_user`

Delete a registered user by username.

**Request Body:**
```json
{
  "username": "john_doe"
}
```

**Responses:**
| Status | Body |
|---|---|
| `200` | `{ "status": "User deleted successfully" }` |
| `400` | `{ "status": "Username is required" }` |

---

### `GET /profile`

Retrieve the current user's profile.

**Response:**
```json
{
  "username": "john_doe",
  "email": "john_doe@example.com"
}
```

---

### `PUT /profile`

Update the current user's profile.

**Request Body:**
```json
{
  "username": "new_name",
  "email": "new_email@example.com"
}
```

**Response:**
```json
{
  "status": "Profile updated successfully"
}
```

---

## 🛡️ Anti-Spoofing Details

The login pipeline enforces **two independent liveness checks** before any face matching is attempted. Both checks must pass or the request is rejected as a spoofing attempt.

### 1. Blink Detection (Eye Aspect Ratio)

The Eye Aspect Ratio (EAR) measures the openness of the eye using six facial landmark points:

```
EAR = (‖p2 − p6‖ + ‖p3 − p5‖) / (2 × ‖p1 − p4‖)
```

- When the eye is open, EAR is approximately **0.3**.
- When the eye blinks, EAR drops sharply below **0.25**.
- Both eyes are measured and averaged.

A detected blink (EAR < 0.25) confirms the subject is a live person, not a static image.

### 2. Head Movement Detection (Pose Angle)

Using 68 facial landmark points from dlib, the system computes the vertical angle between the eye midpoint and the mouth center. If the angle falls within **±20°** of the frontal plane, it detects natural head movement — ruling out a flat, printed photo.

### Why Two Checks?

A single check can be bypassed (e.g., a video loop may contain blinks). Requiring both blink *and* pose verification significantly raises the difficulty of a spoofing attack.

---

## 📁 Project Structure

```
biometric-app-new/
├── run.py                              # Flask backend (API + anti-spoofing logic)
├── shape_predictor_68_face_landmarks.dat  # dlib 68-point landmark model
├── how_to_run                          # Quick-start instructions
├── package.json                        # React app metadata & scripts
├── package-lock.json                   # Locked dependency tree
├── .gitignore                          # Ignored files
├── README.md                           # This file
├── user_data/                          # Auto-created at runtime
│   └── users.json                      # Registered user data (face encodings)
└── anti-spoofing-face-recognition/     # React frontend source
```

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the repository
2. Create a new branch: `git checkout -b feature/your-feature-name`
3. Make your changes and commit: `git commit -m "feat: add your feature"`
4. Push to your fork: `git push origin feature/your-feature-name`
5. Open a Pull Request

Please follow the [Conventional Commits](https://www.conventionalcommits.org/) specification for commit messages.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

Made with ❤️ by [RaptorBlingx](https://github.com/RaptorBlingx)

</div>
