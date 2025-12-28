# Copilot Instructions for Emily AI

## Project Overview

**Emily AI** is an empathetic, browser-based voice companion built with vanilla HTML/CSS/JavaScript. It's a personal assistant focused on emotional support, diary tracking, and voice interaction with no backend dependencies—everything runs client-side.

**Key Fact**: Multiple self-contained HTML files (no build process), with `Emily 4.5.0` being the latest production version. The `HelloAndroid/` directory is a separate Kotlin/Gradle Android subproject.

---

## Architecture & Module Patterns

### Single-File HTML Structure
- Each version is **one self-contained HTML file** with inline `<style>` and `<script>` tags
- No separate JS/CSS files (except HelloAndroid)
- **Two main variants**:
  - **Desktop** (Emily 4.5.0, Emily 4.0.9): Full-featured, TensorFlow.js + Tailwind CDN
  - **Mobile** (Emily Mobile 0.1/0.2/0.3): Tab-based navigation, optimized for touch

### IIFE Module Pattern
All recent versions use JavaScript IIFE (Immediately Invoked Function Expression) for encapsulation:

```javascript
const chatModule = (() => {
  let privateState = [];
  const send = (text) => { /* logic */ };
  return { send }; // Public API only
})();
```

**Core modules** present across versions:
- `chatModule` — Sentiment analysis & response generation
- `voiceModule` — Text-to-speech & speech recognition (Web Speech API)
- `journalModule` — Diary save/load via localStorage
- `visionModule` — Camera access & ML model inference (TensorFlow.js)
- `navModule` (mobile only) — Tab switching logic

### Data Persistence
- **localStorage exclusively** for all persistent data
- Standard keys: `'emilyMemory'`, `'emilyJournal'`, `'diary'`
- Data stored as JSON arrays or plain strings

---

## Critical Technical Patterns

### Media Permission Flow
1. User clicks "Grant Microphone & Camera Access" button
2. `navigator.mediaDevices.getUserMedia({ audio: true, video: true })` triggers browser prompt
3. Stream passed to `initApp(stream)` to activate features
4. Camera optional—app gracefully degrades if denied

**Key detail**: Permissions required for most interactive features. Test this flow first when modifying.

### Machine Learning Integration
- **TensorFlow.js** via CDN: `https://cdn.tensorflow.org/tfjs@latest`
- **Models loaded async** in `initApp()` before feature activation:
  - MobileNet (object classification)
  - PoseNet (pose estimation)
  - face-api.js (face detection & landmarks)
- Models run on `<video>` element canvas; detection results logged to browser console
- **Warning**: Model loading is verbose; expect console logs like "MobileNet model loaded successfully"

### Voice Handling
```javascript
// Find female voice in available system voices
const femaleVoice = voices.find(v => v.name.includes('Female') || v.name.includes('Susan'));
// Synthesize with custom rate & pitch
const utterance = new SpeechSynthesisUtterance(text);
utterance.rate = 0.9; utterance.pitch = 1.1;
speechSynthesis.speak(utterance);
```

### Sentiment/Response Generation
Simple **keyword-based matching** (no external AI):
```javascript
if (text.includes('sad') || text.includes('hurt')) return "I'm here for you...";
if (text.includes('thank')) return "You're welcome, always here for you.";
return randomFromArray(genericReplies); // Fallback
```

---

## Essential Workflows

### Local Development (No Build Process!)
```bash
# Option 1: HTTP Server (required for camera/mic APIs)
npx http-server .
# Then open http://localhost:8080 and select any Emily HTML file

# Option 2: Python
python -m http.server 8000
# Open http://localhost:8000
```

**Important**: Opening HTML files directly via `file://` will **block camera/mic permission APIs**.

### Testing & Debugging
- No automated tests (Jest is in package.json but unused)
- **Browser console is your debugging tool** (`F12` → Console)
- Look for: Media permission logs, model loading messages, chat sentiment analysis
- Vision features log detection results (e.g., "Detected: person, chair, monitor")

### Common Tasks

#### Add a New Response Pattern
Edit the `chatModule`'s `generateResponse()` function:
```javascript
if (text.includes('new_keyword')) return "Emily's new response text";
```

#### Persist New Data
Add a new localStorage key in module init:
```javascript
const load = () => {
  const stored = localStorage.getItem('emilyNewFeature');
  return stored ? JSON.parse(stored) : [];
};
const save = (data) => {
  localStorage.setItem('emilyNewFeature', JSON.stringify(data));
};
```

#### Customize UI Styling
Edit the inline `<style>` tag. Current system uses **Nord color palette** (CSS variables):
- `--deep-blue: #2e3440` (background)
- `--teal: #88c0d0` (accent)
- `--pale-blue: #d8dee9` (text)
- `--aurora-red, --aurora-green, --aurora-purple` (status indicators)

Always use these variables for visual consistency.

---

## Project-Specific Conventions

### File Naming
- Major versions in filename: `Emily 4.5.0`, `Emily Mobile 0.2`
- Experimental/iteration versions append version number: `Emily 3.9.4`, `Emily update 2.4`
- **Latest production**: `Emily 4.5.0` (use as reference for patterns)

### Personality & Tone
Emily is **melancholic, empathetic, reflective**. Her responses:
- Acknowledge user emotions without "fixing" them
- Use soft, patient language ("I'm here for you", "It's okay to feel this way")
- Include idle prompts when chat is inactive: "You ok?", "Just checking in..."
- Contextually reference the user (Jason) and locations (Bendigo, Victoria Street) in some versions

### Code Organization
- **Variables at file scope**: `let journalHandle = null`, `let mobilenetModel = null`
- **DOM elements cached**: `const chatLog = document.getElementById('chatLog')`
- **Event listeners attached** in `initApp()` or `initUI()` after permissions granted
- **No external dependencies except CDN** (Tailwind, TensorFlow, face-api.js)

---

## Integration Points & External Dependencies

### CDN Resources
All loaded in `<head>`:
- **Tailwind CSS**: `https://cdn.tailwindcss.com` (utility-first CSS)
- **TensorFlow.js**: TensorFlow & model libraries (tfjs, mobilenet, posenet, face-api.js)
- **No other imports** (vanilla JS only)

### Browser APIs Required
- **MediaDevices** (`getUserMedia`) — camera/mic access
- **Web Speech API** (`SpeechRecognition`, `speechSynthesis`) — voice I/O
- **localStorage** — persistent storage
- **Canvas API** — vision/ML inference rendering

### Known Limitations (Stubs/Placeholders)
- No backend integration (all responses client-side)
- Emotion analysis is keyword-based, not ML-powered
- Vision detection models load but logic is minimal (detection logged, not used in responses)
- Speech recognition is a Web Speech API placeholder (can be enhanced)

---

## Before Enhancing the Codebase

1. **Test media permissions first**—if this breaks, most features fail
2. **Maintain single-file structure** (unless creating new variant)
3. **Use IIFE modules** for new functionality (preserve state encapsulation)
4. **Add localStorage keys** with clear prefixes (`'emily*'`)
5. **Use Nord colors** for UI consistency
6. **Log to console** (existing codebase is verbose; continue the pattern)
7. **Test in both desktop & mobile** (Emily 4.5.0 for desktop, Emily Mobile 0.3 for mobile)

---

## Key Files to Study

- **[Emily 4.5.0](Emily 4.5.0)** — Production desktop version; TensorFlow integration, full feature set
- **[Emily Mobile 0.2](Emily Mobile 0.2)** — Best modular architecture example; tab-based nav pattern
- **[Emily 4.0.9](Emily 4.0.9)** — Reference implementation; state management approach
- **[README.md](README.md)** — High-level vision and feature descriptions
- **[.github/workflows/Test.yml](.github/workflows/Test.yml)** — CI/CD (minimal; mostly placeholder)

---

**Last Updated**: December 29, 2025  
**Codebase Type**: Single-file vanilla JS + HTML (no build tools)  
**Primary Languages**: HTML5, Vanilla JavaScript, Tailwind CSS  
**Browser APIs**: MediaDevices, Web Speech, Canvas, localStorage
