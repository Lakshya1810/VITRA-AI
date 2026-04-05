# VITRA AI

VITRA AI is a full-stack digital twin app where users can create a personalized AI profile, chat with it, track daily behavior, store memory, and connect services like Google Calendar and Spotify.

Live app: `https://vitra-ai.vercel.app/`

## What The Project Does

- User authentication with email/password and Google sign-in
- Digital twin setup with personality, tone, knowledge, goals, and avatar data
- Persistent chat sessions with stored messages and feedback
- Behavioral tracking for sleep, work, study, mood, and notes
- AI-powered insights and recommendations
- Memory storage for conversations and daily logs
- Google Calendar integration
- Spotify connection and playback controls
- Optional local AI chat through Ollama
- Real-time message events with Socket.IO

## Tech Stack

- Frontend: React 19, TypeScript, Vite, React Router, Tailwind CSS
- Backend: Express, TypeScript, Socket.IO
- Database: MongoDB with Mongoose
- AI: Google Gemini, optional Ollama for local models
- Auth and integrations: JWT, Google OAuth, Spotify API, Nodemailer

## Project Structure

```text
VITRA-AI/
|-- src/                     # React app
|   |-- components/          # Reusable UI pieces
|   |-- context/             # Auth, tutorial, toast providers
|   |-- pages/               # Landing, login, register, chat, dashboard, tracker, insights
|   |-- services/            # Gemini and local AI client logic
|   |-- App.tsx              # Routes and app providers
|   |-- constants.ts         # Frontend constants
|   `-- main.tsx             # React entry
|-- server/                  # Express API modules
|   |-- config/              # MongoDB connection
|   |-- controllers/         # Auth, twins, sessions, messages, memory, connectors
|   |-- middleware/          # Auth and DB guards
|   |-- models/              # Mongoose models
|   |-- routes/              # API route definitions
|   `-- services/            # Connector helpers
|-- server.ts                # Main full-stack server entrypoint
|-- .env.example             # Environment template
|-- .env.local               # Local env file used by the server
|-- package.json             # Scripts and dependencies
|-- vite.config.ts
`-- README.md
```

## How It Runs

This project currently runs as a single Node process:

- `server.ts` starts the Express server on port `3000`
- In development, Vite is mounted in middleware mode and serves the React app
- In production, Express serves the built files from `dist/`

That means local development uses one command and one port instead of separate frontend/backend servers.

## Main Routes And Features

Frontend pages include:

- `/` landing page
- `/login`, `/register`
- `/forgot-password`, `/reset-password`
- `/dashboard`
- `/setup`
- `/chat`
- `/tracker`
- `/insights`

Backend API areas include:

- `/api/auth` for register, login, Google auth, password reset, current user
- `/api/twins` for twin profile, system prompt generation, feedback
- `/api/sessions` for chat sessions and messages
- `/api/daily-data` for behavioral logs
- `/api/memory` for stored memory data
- `/api/connect` for Google Calendar and Spotify flows
- `/api/chat/local` for Ollama-backed local chat
- `/api/insights/analyze` for lightweight non-Gemini analysis
- `/api/health` for server health status

## Getting Started

### Prerequisites

- Node.js 18+
- npm
- MongoDB connection string

Optional:

- Gemini API key for cloud AI features
- Google OAuth credentials
- Spotify app credentials
- Gmail app password for reset emails
- Ollama installed locally if you want local model chat

### Install

```bash
npm install
```

### Configure Environment

Copy `.env.example` to `.env.local` and fill in your values.

```bash
cp .env.example .env.local
```

Required or commonly used variables:

```env
MONGODB_URI=your_mongodb_uri
JWT_SECRET=your_jwt_secret
GEMINI_API_KEY=your_gemini_api_key
GOOGLE_CLIENT_ID=your_google_client_id
VITE_GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
SPOTIFY_CLIENT_ID=your_spotify_client_id
SPOTIFY_CLIENT_SECRET=your_spotify_client_secret
APP_URL=http://localhost:3000
EMAIL_SERVICE=gmail
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_app_password
EMAIL_FROM="VITRA Support <your_email@gmail.com>"
```

Notes:

- `server.ts` loads `.env.local` first, then falls back to normal environment variables
- The frontend uses same-origin API calls, so there is no `VITE_API_URL` requirement in the current setup
- Google sign-in on the client uses `VITE_GOOGLE_CLIENT_ID`
- If MongoDB is not reachable, the UI shows a database warning banner

### Run In Development

```bash
npm run dev
```

Open `http://localhost:3000`

## Scripts

```bash
npm run dev      # Start the full-stack app with tsx
npm run start    # Start the same server entrypoint
npm run build    # Build the frontend to dist/
npm run preview  # Preview the Vite frontend only
npm run lint     # Type-check the project
```

`npm run preview` is useful for checking the built frontend, but it does not replace the Express API server.

## AI Modes

### Gemini

Gemini powers features like:

- twin chat responses
- recommendations
- mood prediction
- avatar generation
- text-to-speech
- quick suggestions
- trait extraction and memory summarization

These features require `GEMINI_API_KEY`.

### Local AI With Ollama

The app also supports local chat through Ollama:

- `GET /api/chat/local/models`
- `POST /api/chat/local`

If Ollama is not running on `localhost:11434`, the server returns a helpful error with a suggestion.

## Integrations

### Google OAuth / Calendar

Used for:

- Google sign-in
- Google Calendar connector flow

Make sure your Google Cloud app includes the correct authorized origins and redirect URIs for your deployment.

### Spotify

Used for:

- Spotify account connection
- playback status
- playback control actions from the app

## Deployment Notes

Current production links in the project:

- Frontend: `https://vitra-ai.vercel.app/`
- Backend/API: `https://vitra-backend.onrender.com`

The local repository is structured as a single app, but the deployed version may still use a split frontend/backend setup. If you deploy it yourself, keep environment variables and OAuth redirect URLs aligned with your chosen architecture.

## Known Implementation Notes

- The server currently uses port `3000` directly inside [`server.ts`](/f:/VITRA/VITRA-AI/server.ts)
- Same-origin API requests are configured in [`src/constants.ts`](/f:/VITRA/VITRA-AI/src/constants.ts)
- Auth state and DB health handling live in [`src/context/AuthContext.tsx`](/f:/VITRA/VITRA-AI/src/context/AuthContext.tsx)
- The frontend route tree is defined in [`src/App.tsx`](/f:/VITRA/VITRA-AI/src/App.tsx)

## License

No license is currently defined in this repository.
