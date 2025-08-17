# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a TTS (Text-to-Speech) tool that creates an OpenAI.fm clone. It's a full-stack application with a React frontend and Node.js backend for generating text-to-speech audio using OpenAI's TTS API.

## Architecture

- **Frontend**: React + TypeScript + Vite (client/ directory)
- **Backend**: Node.js + Express (server/ directory) 
- **Audio Storage**: Local `/audio` directory for generated MP3 files
- **Dependencies**: OpenAI API (returns MP3 natively), CORS for API communication

The application follows a monorepo structure with separate client and server packages.

## Development Commands

### Frontend (client/)
```bash
cd client
npm run dev        # Start Vite dev server
npm run build      # Build for production (tsc -b && vite build)
npm run lint       # Run ESLint
npm run preview    # Preview production build
```

### Backend (server/)
```bash
cd server
# No dev scripts configured yet - needs nodemon setup
```

### Root level
```bash
npm run dev        # Will use concurrently to run both client and server (when implemented)
```

## Key Implementation Details

### Backend Requirements
- Express server with CORS enabled
- OpenAI API integration for TTS generation (returns MP3 directly)
- File system operations for saving audio to `/audio` directory
- API endpoints for:
  - POST `/api/generate` - Generate TTS audio
  - GET `/api/presets` - Get style presets
  - PUT `/api/presets/:id` - Update presets
  - POST `/api/presets/reset/:id` - Reset presets

### Frontend Features
- Single-page interface with unlimited text input
- Voice selection (Alloy, Echo, Fable, Nova, Onyx, Shimmer)
- Professional style presets (editable and persistent)
- Audio player for preview and download
- Character counter and status displays

### Configuration Files
- `server/config.js` - OpenAI API key and settings
- `server/presets.json` - Professional style presets (10 predefined styles)

### Audio File Naming
Generated files use timestamp format: `audio_YYYYMMDD_HHMMSS.mp3`

## Current State

The project appears to be in initial setup phase:
- Client has basic Vite + React template
- Server directory exists but lacks implementation
- No main development script configured yet
- Root package.json only has concurrently as dependency

Refer to `docs/prd.md` for complete feature specifications and implementation details.