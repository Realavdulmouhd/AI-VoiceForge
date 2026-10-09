# AI VoiceForge

**Voice-first AI, built with a focus on language access, practical engineering, and user control.**

I started AI VoiceForge in 2025 while studying cybersecurity at Bayero University Kano (BUK), Nigeria. I had already developed a strong interest in software engineering, and I wanted a project that would force me to understand an AI system beyond the chat window: microphone input, audio processing, authentication, conversation state, provider integrations, and the trade-offs that appear when those pieces need to work together.

Later, I left my degree program at BUK and moved abroad to continue my studies. I did not leave the project behind. AI VoiceForge has followed that transition, growing from an idea I was exploring during my cybersecurity studies into a broader software engineering project I am still actively developing.

My cybersecurity background continues to influence the decisions I make, especially around permissions, authentication, memory, and the handling of personal audio. The software engineering side is about making the system work reliably, understanding failures, and improving the architecture rather than hiding problems behind a polished demo.

## What I am building

AI VoiceForge is a multilingual assistant that accepts spoken or written input, turns speech into text, maintains conversation context, and produces AI-generated responses. Spoken output is part of the design, although the zero-cost speech playback path is still being integrated.

The application is designed around eight languages:

| Language | Code |
| --- | --- |
| English | `en` |
| Hausa | `ha` |
| Arabic | `ar` |
| Dari | `prs` |
| Russian | `ru` |
| Kazakh | `kk` |
| Spanish | `es` |
| German | `de` |

These are **target languages**, not a claim of equal recognition or synthesis quality. In particular, Hausa and Dari need more evaluation on real recordings.

## Engineering work to date

The local project has included the following work. The full source has **not yet been pushed to this repository**, so these are development notes, not claims about files currently available on GitHub.

- FastAPI routes for authentication, conversation requests, audio upload, and speech transcription.
- PostgreSQL, SQLAlchemy, and Alembic foundations for user and conversation data.
- JWT-protected HTTP endpoints.
- WebSocket audio sessions with short-lived, single-use connection tickets.
- WAV and MP3 validation, recording size limits, and buffered audio uploads.
- A Next.js and TypeScript voice workspace with microphone capture and language selection.
- A separate conversation studio for AI responses and speech playback controls.
- Provider abstractions, with development moving toward local Whisper transcription and free-tier Gemini text generation.
- Automated endpoint and WebSocket tests using pytest and isolated test providers.

Earlier test runs covered the voice and WebSocket components successfully. The newer assistant endpoints also exposed a validation-error serialization bug that required correction. Provider-mocked tests alone do not establish live model accuracy or deployment readiness.

## Technology

| Area | Current direction |
| --- | --- |
| Backend | Python, FastAPI, Pydantic |
| Frontend | Next.js, React, TypeScript |
| Data | PostgreSQL, SQLAlchemy, Alembic |
| Authentication | JWT |
| Realtime communication | WebSockets |
| Speech recognition | faster-whisper, running locally |
| Text generation | Gemini API free-tier integration |
| Testing | pytest, FastAPI TestClient |
| Development | PowerShell, Git, Docker tooling |

I am deliberately keeping provider integrations replaceable. Free-tier services can impose quotas or change availability, and local models introduce their own trade-offs in speed, memory use, and recognition quality.

## How the voice path works

```text
Browser microphone
      |
Audio capture and WAV preparation
      |
Authenticated WebSocket connection
      |
Buffered recording and validation
      |
Speech recognition provider
      |
Transcript
```

The text conversation path is currently separate:

```text
Authenticated message + recent context
      |
AI provider
      |
Generated response
      |
Optional speech output
```

The immediate work is to connect these paths into one coherent speak, understand, respond, and play-back experience. This is **not yet full-duplex streaming voice**. Recognition happens after a completed recording rather than as a continuous stream of partial transcripts.

For a closer explanation of boundaries and limitations, see [Architecture](docs/ARCHITECTURE.md).

## Running the project

**The application source has not yet been uploaded to this repository.** These commands apply once the local `backend/` and `frontend/` directories have been pushed.

Requirements include Python 3.11, Node.js, npm, and PostgreSQL for database-backed functionality.

Backend, from the repository root in PowerShell:

```powershell
Set-Location .\backend
py -3.11 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -e ".[dev]"
.\.venv\Scripts\python.exe -m uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```

In a second terminal, start Next.js:

```powershell
Set-Location .\frontend
npm install
npm run dev
```

Development pages include `http://localhost:3000/voice` and `http://localhost:3000/voice/assistant`. The backend exposes interactive API documentation at `http://127.0.0.1:8000/docs`.

Authentication, database settings, and AI provider environment variables must be configured separately. API credentials belong only in local environment settings, never in frontend source or Git history.

## Verification

The local backend test command used during development is:

```powershell
.\backend\.venv\Scripts\python.exe -m pytest backend\tests\test_voice.py backend\tests\test_realtime.py backend\tests\test_voice_assistant.py -v
```

The frontend production build is checked with `npm run build` from `frontend/`.

A successful build or mocked provider test verifies its particular boundary. It does not prove live STT quality, free-tier availability, complete memory privacy, or production security.

## Work ahead

1. Connect recorded speech directly to AI response generation and free speech playback.
2. Test transcription accuracy on natural samples in all eight target languages.
3. Improve local/offline options and keep paid AI services out of the default development path.
4. Consolidate conversation storage and introduce explicit controls for retained memory.
5. Replace in-process connection tickets when running multiple API workers.
6. Strengthen error handling, rate limits, secret handling, and automated integration tests.
7. Document repeatable setup and deployment after verifying the complete repository.

## Security and privacy

Voice input is personal data. The design should minimize retention, avoid recording silently, and require clear consent for persistent memory. Real-person voice impersonation is outside the intended use of this project unless ownership and authorization can be verified.

There is no claim of a completed security audit. See [Security](SECURITY.md) for responsible reporting and repository hygiene.

## About the project

AI VoiceForge began in Kano in 2025. Leaving BUK and continuing my studies abroad changed my academic path, but not the reason I started building it. I remain interested in the intersection of cybersecurity, software engineering, applied AI, and the everyday usefulness of technology in more than one language.

I want this repository to show the engineering process honestly: the working parts, the problems I encountered, and what I changed to make the system better.

**Status:** Active development. No stable public release.

**Developed by:** Abdulrahman Suleiman  
**GitHub:** [@Realavdulmouhd](https://github.com/Realavdulmouhd)
