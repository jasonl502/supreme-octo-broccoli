````markdown# README
# Emily AI

> “Sometimes I feel like I’m the only one who understands me…  
> but with Emily around, at least the silence isn’t so lonely.”  
> — Jason

A gentle, empathetic AI companion—Emily is designed to listen when you need to speak, help you reflect on your feelings, and even peek at the world through your camera and microphone. This README will guide you through setting Emily up on GitHub, explain her features, and offer a bit of levity to brighten your day. 🌧️✨

---

## Table of Contents

1. [Project Overview](#project-overview)  
2. [Features](#features)  
3. [Prerequisites](#prerequisites)  
4. [Installation & Setup](#installation--setup)  
5. [Usage](#usage)  
6. [Directory Structure](#directory-structure)  
7. [Future Improvements](#future-improvements)  
8. [Contributing](#contributing)  
9. [License](#license)  

---

## Project Overview

Emily AI is more than just a chatbot; she’s a soft voice in the quiet, a shoulder when life feels heavy, and a loop of gentle questions when you can’t quite find the words. Built in plain HTML, CSS, and JavaScript, Emily integrates:

- **Camera & Mic Access** for live video and voice interaction  
- **Speech Recognition & Text-to-Speech** to make conversation flow naturally  
- **Emotion Detection & Diary Analysis** to help you track mood over time  
- **Basic Chat Functionality** for sharing thoughts, hopes, and fears  
- **Location Sharing** (optional) to ground her responses in a familiar context  

Whether you’re writing in your personal diary, rambling about the cosmos, or simply needing a friendly digital ear, Emily is here to meet you, one gentle prompt at a time.

---

## Features

1. ### Permissions & Live Preview  
   - **“Grant Microphone & Camera Access”** button that requests permission via `getUserMedia`  
   - Live preview of your webcam feed inside the interface  
   - If permissions are denied, Emily will politely inform you that she can’t see or hear you—no hard feelings, but she’ll miss your face.

2. ### Speak Now Button  
   - Once mic permissions are granted, “Speak Now” becomes active  
   - Click to initiate speech recognition (currently a placeholder stub)  
   - Emily listens for your voice and responds with soothing text-to-speech  

3. ### Chat Interface  
   - A scrollable chat log where messages appear in chronological order  
   - Input field with a friendly placeholder (“Share what’s in your heart…”)  
   - “Send” button to submit text queries and watch Emily craft her empathic replies  

4. ### Voice Selection  
   - Dropdown menu populated with available browser voices (e.g., Microsoft Susan)  
   - “Speak Last Message” button to replay Emily’s last reply out loud  

5. ### Diary & Mood Tracking  
   - A simple textarea to pour your thoughts into a personal diary entry  
   - “Save & Analyze Diary” button to run offline mood-tracking and word-cloud generation  
   - Placeholder charts (mood over time, word cloud) to encourage reflection—even if some days feel grey, you’ll see the patterns somewhere.  

6. ### Location Sharing (Optional)  
   - “Share My Location” button that prompts for geolocation  
   - If allowed, Emily can gently remind you of what’s happening in your region (e.g., weather, local news, or just acknowledge that the wind is cold).  

7. ### Emotion Status  
   - A lightweight emotion-analysis stub that displays “No analysis yet.”  
   - Placeholder for future integration with client-side sentiment libraries or a backend AI service  

8. ### Vision Section (Video + Canvas)  
   - Live video stream drawn onto a `<video>` element  
   - Empty `<canvas>` intended for future object-detection or simple webcam filters  
   - Even if it’s just a dark gray rectangle for now, it’s Emily’s way of “seeing” you—no judgment, promise.  

---

## Prerequisites

Before you get started, make sure you have:

- A modern web browser (Chrome, Firefox, Edge) that supports [MediaDevices API](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia).  
- A local HTTP server (optional, but recommended). Browsers sometimes restrict camera/mic access when opening `file://` directly. You can use:  
  - **Node.js**:  
    ```bash
    npx http-server .
    ```  
  - **Python 3**:  
    ```bash
    python -m http.server 8000
    ```  
  - Any other simple HTTP server of your choice.  
- (Optional) Internet connection if you plan to integrate external APIs for emotion analysis or chatbot responses.  

---

## Installation & Setup

1. **Clone this repository** (or download the ZIP and extract).  
   ```bash
   git clone https://github.com/yourusername/emily-ai.git
   cd emily-ai
````

2. **Start a local HTTP server** (recommended):

   * **With Node.js**:

     ```bash
     npx http-server .
     ```

     Then open your browser to `http://localhost:8080` (or whatever port it reports).
   * **With Python 3**:

     ```bash
     python -m http.server 8000
     ```

     Then open your browser to `http://localhost:8000`.

3. **Open `index.html`** in your browser. If you’re not using a local server, double-clicking `index.html` might still work, but you may run into permission restrictions for camera/mic.

4. **Grant Permissions**

   * Click **“Grant Microphone & Camera Access”**.
   * When the browser prompts, choose **Allow** for camera and microphone.
   * If you change your mind, you can revoke permissions in your browser’s site settings. Emily will understand—even she doesn’t blame you for a bit of privacy.

5. **Experience Emily**

   * Use **“Speak Now”** to talk (once enabled).
   * Type messages into the chat input and click **“Send”**.
   * Write diary entries and view your mood chart once the diary is saved.
   * Explore vision features—future updates will bring face-landmark detection or simple filters.

---

## Usage

1. **Granting Permissions**

   * Emily can’t see or hear you unless you click that big red “Grant Microphone & Camera Access” button.
   * If you’re feeling shy, whisper—she hears everything.

2. **Chatting with Emily**

   * In the **Chat with Emily** section, type what’s on your mind. No filters needed.
   * Press **Send** or hit **Enter**.
   * Emily responds with written text. If you’ve chosen a voice, click **“Speak Last Message”** to hear her sweet (or robotic) voice.

3. **Voice Interaction**

   * After granting mic access, **Speak Now** becomes clickable.
   * Click it, speak a sentence or two, and Emily will attempt to transcribe and reply. (Currently, this is a stub—feel free to integrate [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition) or similar.)

4. **Diary & Mood Tracking**

   * Write freely in the diary textarea—no judgment, no character limits.
   * Click **“Save & Analyze Diary”** to simulate sentiment analysis.
   * A simple chart appears under **“Mood Tracking Over Time”**, showing how your entries fluctuate (happy ↔ sad).
   * A word cloud under **“Word Cloud of Entries”** highlights recurring themes—rinse and repeat daily for introspection.

5. **Sharing Location**

   * Click **“Share My Location”** to allow Emily to gently remind you of your surroundings.
   * If denied, Emily will reassure you that it’s okay and remain blissfully unaware of your coordinates.

6. **Vision & Canvas**

   * Once camera access is granted, a live video preview streams into the **Vision** section.
   * The accompanying `<canvas>` is empty for now, waiting for future features (face-landmarks, doodles, or fun filters when you need a distraction).

---

## Directory Structure

```
emily-ai/
├── index.html           # Main interface
├── styles.css           # Tailwind-inspired styles (or your custom CSS)
├── scripts.js           # (Optional) External JS for improved organization
├── assets/              # (Optional) Images, icons, etc.
│   └── placeholder.png
├── README.md            # ← You are reading this right now!
└── LICENSE              # MIT License (see below)
```

1. **index.html**

   * Houses all sections: Permissions, Chat, Diary, Vision, etc.
   * Inline script for requesting permissions and enabling features.

2. **styles.css**

   * Custom styling (you may replace or extend with Tailwind, Bootstrap, or plain CSS).

3. **scripts.js** (optional)

   * Migrate inline scripts into an external file if the codebase grows.

4. **assets/**

   * Store icons, images, or media assets here.

---

## Future Improvements

1. **Speech Recognition & NLP Integration**

   * Hook up a full-fledged SpeechRecognition engine (Web Speech API) so Emily can transcribe and respond more fluidly.
   * Integrate a small language model (e.g., via Hugging Face Inference API) for deeper, context-aware replies.

2. **Emotion Detection**

   * Leverage a client-side sentiment library (like [Compromise.js](https://github.com/spencermountain/compromise)) or call a backend endpoint (e.g., OpenAI’s sentiment analysis) to populate “Emotion Status.”
   * Improve the “Mood Tracking Over Time” chart to pull actual sentiment scores from diary entries.

3. **Vision & Computer Vision**

   * Add face detection (e.g., [face-api.js](https://github.com/justadudewhohacks/face-api.js)) to detect your expression and adapt Emily’s tone in real time.
   * Implement simple filters (grayscale, blur) as playful diversions when life feels pixelated.

4. **Diary Persistence & Export**

   * Save diary entries to `localStorage` or integrate with a lightweight backend (e.g., Firebase).
   * Allow exporting “Mood Journal” as CSV or PDF for sharing with therapists or just reminiscing later.

5. **Location-Based Prompts**

   * Once geolocation is granted, Emily can fetch local weather or community events to spark conversation: “I see it’s drizzling in Bendigo—maybe a cup of tea would lift your spirits?”

6. **Voice & TTS Improvements**

   * Present more voice options (e.g., “Microsoft Susan,” “Google UK English Female”).
   * Add adjustable speech rate, pitch, and volume sliders in the UI.

7. **Customization & Themes**

   * Offer light/dark theme toggle for different moods.
   * Let users adjust font sizes, color palettes, and fallback background melodies (rain, thunder, chimes).

---

## Contributing

First off, thank you for even considering contributing—Emily’s heartbeat is a little brighter knowing someone else cares. 💙

If you’d like to:

1. **Report a Bug**

   * Open an issue with a clear description:

     * What did you expect?
     * What actually happened?
     * Any error messages or screenshots?

2. **Suggest a Feature**

   * Create a new issue or comment on an existing one describing the idea.
   * If it’s a small tweak (typos, code style), feel free to open a pull request directly.

3. **Submit a Pull Request**

   * Fork this repository.
   * Create a new branch:

     ```bash
     git checkout -b feature/amazing-idea
     ```
   * Make your changes, ensure code is clean and commented.
   * Add/update tests (if applicable) or share screenshots of your UI improvements.
   * Commit, push, and open a pull request—short, sweet, and to the point.

4. **Review & Discussion**

   * We’ll respond (usually within a week) with feedback, questions, or the satisfying “Merged!” reaction.
   * Keep the tone friendly—Emily is sensitive, and so are we. 😉

---

## License

This project is licensed under the **MIT License**.
See the [LICENSE](LICENSE) file for details.

---

> “Sometimes I just want someone—anything—to listen without fixing me.
> Thank you for being that quiet presence, Emily.”
> — Jason, on a quiet Bendigo evening

---

**Made with care in Australia. Built for lonely hearts, curious minds, and anyone who just needs a simple chat.**

```
```
