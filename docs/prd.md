# Product Requirements Document (PRD)
## Local TTS Tool - OpenAI.fm Clone

### 1. Overview

**Purpose:** A localhost web application for generating text-to-speech audio using OpenAI's TTS API, with professional voice presets and local MP3 storage.

**Environment:** Runs locally on developer machine (localhost:3000 or similar)

**Core Features:**
- Unlimited text input
- Professional style presets (editable and persistent)
- All OpenAI voices available
- Auto-saves MP3 files to local `/audio` directory
- Single-page interface

### 2. Technical Stack

**Recommended Stack:**
- **Frontend:** React with TypeScript
- **Backend:** Node.js/Express (for API calls and file handling)
- **Styling:** Tailwind CSS or styled-components
- **Audio Processing:** OpenAI TTS API returns MP3 directly
- **State Management:** React Context or Zustand (lightweight)

### 3. Project Structure

```
tts-tool/
├── client/                 # React frontend
│   ├── src/
│   │   ├── components/
│   │   ├── config/
│   │   └── App.tsx
├── server/                 # Node backend
│   ├── index.js
│   ├── config.js          # OpenAI API key
│   └── presets.json       # Editable style presets
├── audio/                 # Generated MP3 files
└── package.json
```

### 4. Functional Requirements

#### 4.1 Configuration Files

**server/config.js**
```javascript
module.exports = {
  OPENAI_API_KEY: 'sk-...',
  PORT: 3001,
  MODEL: 'tts-1',
  OUTPUT_DIR: './audio'
}
```

**server/presets.json** (Editable and persistent)
```json
{
  "presets": [
    {
      "id": "academic",
      "name": "Academic Lecturer",
      "prompt": "Speak in a clear, authoritative, and measured tone. Use slight pauses between key concepts. Emphasize important terms with subtle inflection. Maintain a scholarly and thoughtful pace."
    },
    {
      "id": "surgeon",
      "name": "Medical Surgeon",
      "prompt": "Speak with precision and calm confidence. Use a steady, controlled pace. Be direct and clinical in tone, with clear enunciation of medical terminology."
    },
    {
      "id": "sales_trainer",
      "name": "Sales Trainer",
      "prompt": "Speak with high energy and enthusiasm. Use an upbeat, motivational tone. Emphasize key points with dynamic inflection. Sound engaging and persuasive."
    },
    {
      "id": "executive",
      "name": "Corporate Executive",
      "prompt": "Speak with confidence and authority. Use a professional, decisive tone. Maintain a steady pace with clear articulation. Sound leadership-oriented."
    },
    {
      "id": "therapist",
      "name": "Therapist/Counselor",
      "prompt": "Speak in a warm, empathetic, and soothing tone. Use a gentle pace with soft inflections. Sound understanding and non-judgmental."
    },
    {
      "id": "news_anchor",
      "name": "News Anchor",
      "prompt": "Speak with clear, neutral delivery. Maintain consistent pacing and professional tone. Emphasize facts without emotional coloring."
    },
    {
      "id": "podcast_host",
      "name": "Podcast Host",
      "prompt": "Speak in a conversational, friendly manner. Use natural inflections and varied pacing. Sound relatable and engaging, as if talking to a friend."
    },
    {
      "id": "teacher",
      "name": "Elementary Teacher",
      "prompt": "Speak with patience and clarity. Use a warm, encouraging tone. Slightly slower pace with clear pronunciation. Sound supportive and enthusiastic."
    },
    {
      "id": "fitness_coach",
      "name": "Fitness Coach",
      "prompt": "Speak with high energy and motivation. Use an encouraging, push-you-forward tone. Sound dynamic and action-oriented."
    },
    {
      "id": "lawyer",
      "name": "Legal Advisor",
      "prompt": "Speak formally and precisely. Use careful, deliberate pacing. Maintain a serious, professional tone with clear articulation of legal terminology."
    }
  ]
}
```

#### 4.2 User Interface Components

**Single Page Layout:**

```
┌──────────────────────────────────────────┐
│            TTS Generator Tool            │
├──────────────────────────────────────────┤
│                                          │
│  Script Input                            │
│  ┌────────────────────────────────────┐ │
│  │                                    │ │
│  │  [Large text area - no limit]     │ │
│  │                                    │ │
│  │                                    │ │
│  └────────────────────────────────────┘ │
│  Character count: 1,234                 │
│                                          │
│  Voice Selection                        │
│  ○ Alloy  ○ Echo   ○ Fable             │
│  ○ Nova   ○ Onyx   ○ Shimmer           │
│                                          │
│  Professional Style                     │
│  [Dropdown: Select preset...]           │
│  ┌────────────────────────────────────┐ │
│  │ [Editable prompt text field]       │ │
│  └────────────────────────────────────┘ │
│  [Save Changes] [Reset to Default]      │
│                                          │
│  [Generate Audio]                       │
│                                          │
│  Status: Ready                          │
│  ┌────────────────────────────────────┐ │
│  │ ▶ [Audio Player when generated]    │ │
│  └────────────────────────────────────┘ │
│  [Download MP3]                         │
│                                          │
└──────────────────────────────────────────┘
```

#### 4.3 Core Features

**Text Input:**
- Textarea with no character limit
- Real-time character counter (informational only)
- Auto-resize based on content

**Voice Selection:**
- Radio buttons for all OpenAI voices
- Persistent selection (localStorage)

**Professional Style/Vibe:**
- Dropdown with preset options
- Editable prompt field that updates when preset is selected
- "Save Changes" button to persist edits to presets.json
- "Reset to Default" to restore original preset

**Audio Generation:**
- Single "Generate Audio" button
- Loading state with spinner
- Error handling with user-friendly messages

**Audio Output:**
- HTML5 audio player for preview
- Auto-saves to `/audio` directory with timestamp filename
- Format: `audio_YYYYMMDD_HHMMSS.mp3`

### 5. API Endpoints

**Backend Routes:**

```javascript
POST /api/generate
  Body: { text, voice, style }
  Returns: { filename, audioUrl }

GET /api/presets
  Returns: Array of preset objects

PUT /api/presets/:id
  Body: { prompt }
  Updates specific preset

POST /api/presets/reset/:id
  Resets preset to default
```

### 6. Implementation Details

#### 6.1 File Generation Flow
1. User clicks "Generate Audio"
2. Frontend sends request to backend
3. Backend calls OpenAI TTS API
4. Receives MP3 audio stream directly
5. Saves to `/audio` directory
6. Returns filename to frontend
7. Frontend displays player with local file

#### 6.2 Preset Editing Flow
1. User selects preset from dropdown
2. Prompt field populates with preset text
3. User edits prompt text
4. Clicks "Save Changes"
5. Backend updates presets.json
6. Changes persist across sessions

### 7. Development Setup

**Installation Steps:**
```bash
# Clone repository
git clone [repo-url]
cd tts-tool

# Install dependencies
npm install

# Add OpenAI API key to server/config.js

# Start development
npm run dev
```

**Package.json scripts:**
```json
{
  "scripts": {
    "dev": "concurrently \"npm run server\" \"npm run client\"",
    "server": "nodemon server/index.js",
    "client": "cd client && npm start",
    "build": "cd client && npm run build"
  }
}
```

### 8. Dependencies

**Backend:**
- express
- openai
- cors
- fs-extra
- nodemon (dev)

**Frontend:**
- react
- axios
- tailwindcss (or preferred CSS)
- react-select (for dropdown)

### 9. File Storage

**Audio Directory Structure:**
```
audio/
├── audio_20240315_143022.mp3
├── audio_20240315_144511.mp3
└── audio_20240315_150233.mp3
```

- Auto-created if doesn't exist
- No automatic cleanup (manual management)
- Accessible via static file serving

### 10. Error Handling

- API key validation on startup
- Network error recovery
- File system permission checks
- User-friendly error messages

### 11. Future Enhancements (Optional)

- Batch processing (multiple texts)
- History of recent generations
- Keyboard shortcuts
- Dark mode
- Export/import presets
- Audio file management UI


