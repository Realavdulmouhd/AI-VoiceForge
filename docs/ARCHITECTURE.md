# Architecture

This document describes the architecture developed locally for AI VoiceForge. The source code still needs to be pushed from the development machine. Until that happens, treat this document as a design and implementation record rather than proof that all components are available in this repository.

## Main components

### Backend

- `app/api`: HTTP routes, authentication dependencies, and the WebSocket interface.
- `app/core`: configuration, request logging, and exception handling.
- `app/db`: PostgreSQL data models and database sessions.
- `app/domain`: supported language and conversation concepts.
- `app/providers`: provider contracts and general AI model adapters.
- `app/voice`: audio validation, transcription, conversation responses, and speech providers.
- `app/realtime`: single-use WebSocket tickets and recording state.
- `app/services`: user, conversation, memory, and persistence logic.
- `app/safety`, `app/intelligence`, `app/observability`: earlier experimental components that require consolidation and closer evaluation.

### Frontend

Next.js App Router pages provide microphone capture and conversation controls. The browser sends microphone recordings to FastAPI using the authenticated WebSocket protocol. The conversation studio currently makes separate requests for model-generated responses.

## Recording protocol

```text
JWT-authenticated POST /api/v1/realtime/ticket
               |
         One-use ticket
               |
WebSocket /api/v1/realtime/ws
               |
       Authentication event
               |
          audio.start
               |
      Binary audio chunks
               |
          audio.commit
               |
        WAV/MP3 validation
               |
       STT provider adapter
               |
     transcription.completed
```

The current model handles a completed recording and returns a transcript. It does not emit incremental partial transcripts.

## Conversation responses

The assistant route accepts the latest text message, language, mode, and bounded recent history. A provider produces a response. This is not yet equivalent to validated, server-owned persistent conversation context.

A free-development configuration uses Gemini for text generation where free-tier access is available and faster-whisper for CPU-based speech recognition. Browser speech synthesis is a future zero-cost playback option. Availability and quality can vary.

## Language handling

Target language codes are `en`, `ha`, `ar`, `prs`, `ru`, `kk`, `es`, and `de`. Speech model capability must be tested separately for each language. Dari uses a Persian transcription language hint in the initial provider adapter; that is a practical fallback, not a dedicated Dari model.

## State and data

- The browser keeps short-lived interface state and client-supplied conversation turns.
- WebSocket tickets are single-use and currently held in process memory.
- Voice recordings are buffered for one completed transcription request.
- PostgreSQL models were introduced for user accounts and conversation data.
- Memory and long-term retention rules remain under development.

In-process ticket storage is suitable only for local single-worker development. Multi-worker deployment needs shared atomic storage, expiry, and explicit resource limits.

## Security work remaining

Review authentication, rate limiting, WebSocket origins, data access policies, replay protection across workers, memory deletion, upload validation, and retention before any public service launch.

No audio recording or model output should be treated as inherently trusted.
