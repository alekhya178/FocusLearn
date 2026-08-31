# ZoneIn

**AI-powered focused learning platform** that turns YouTube from a distraction machine into a structured place to study.

Type in a goal, get a roadmap. Watch a video, get real notes and a quiz pulled from its actual transcript. Study on YouTube, and a Chrome extension keeps Focus Mode on and tracks what you actually watched.

🔗 [Live](https://zone-in-two.vercel.app) · [Repo](https://github.com/alekhya178/ZoneIn)

Built for Project Space Season 8 by **Team Mind Barriers** — D. Pravallika, G. Srija, M. Sai Harshitha, Ch. Madhuri, P. Alekhya, P. Thanmahi.

---

## Why

YouTube is great at giving you the first educational video and terrible at giving you the tenth — recommendations drift toward Shorts and entertainment, there's no curriculum, and "watched" isn't the same as "learned." ZoneIn wraps structure, tracking, and AI around YouTube instead of trying to replace it.

## How it works

```
Learning goal → Groq AI → 10-topic roadmap
      ↓
Pick a topic → best-fit video found (YouTube Data API)
      ↓
Study it → transcript pulled (or Whisper-transcribed if unavailable)
      ↓
AI generates notes + a 5-question quiz from that transcript
      ↓
Chrome extension enforces Focus Mode on YouTube, tracks watch time in 5s heartbeats
      ↓
Watch %, quiz score, and study time roll up into a per-topic engagement score
      ↓
Dashboard: weekly/monthly hours, focus-score trend, streaks, 14-day activity heatmap
```

## Architecture

Three apps, one backend:

- **`focused-learning-web/`** — React 19 + Vite dashboard: roadmaps, study sessions, notebook, chatbot, analytics
- **`focused-learning-extension/`** — Chrome MV3 extension: Focus Mode + content filtering on YouTube itself, React popup for login/status
- **`focused-learning-backend/`** — Express API + Socket.io, talks to MongoDB, Groq, and the YouTube Data API

## Stack

| | |
|---|---|
| Frontend | React 19, Vite, Tailwind, Recharts, React Router |
| Backend | Node.js, Express, Mongoose, Socket.io |
| Database | MongoDB Atlas |
| AI | Groq (`llama-3.3-70b-versatile` for roadmaps/notes/quizzes, `whisper-large-v3` as a transcript fallback via ffmpeg) |
| Video data | YouTube Data API v3 (video discovery), `youtube-transcript` (direct transcripts) |
| Auth | Firebase Authentication + Admin SDK, JWT for API sessions, OTP flows for password reset/account deletion |
| Extension | Manifest V3 — background service worker + content script + React popup |
| Deploy | Vercel (web) + Render (API) |

## Getting started

Needs a MongoDB URI, a Firebase project, a Groq API key, and a YouTube Data API key.

```bash
# backend
cd focused-learning-backend
npm install
cp .env.example .env    # fill in MONGO_URI, JWT_SECRET, GROQ_API_KEY, YOUTUBE_API_KEY, Firebase admin creds
npm run dev              # http://localhost:5000

# web app
cd focused-learning-web
npm install
npm run dev               # http://localhost:5173

# extension
cd focused-learning-extension
npm install && npm run build
# chrome://extensions → Developer mode → Load unpacked → select this folder
```

Never commit `.env`, Firebase service-account keys, or API keys.

## Testing

```bash
cd focused-learning-web && npm test        # Vitest + React Testing Library
cd focused-learning-backend && npm test    # Jest + Supertest
```

## What's next

Smarter distraction detection, adaptive roadmaps based on quiz performance, adaptive/weak-topic quizzes, AI-generated revision schedules.

---

*"Learn Smarter. Stay Focused. ZoneIn."*
