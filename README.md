# 🤖 AI Productivity Browser Extension

> An AI-powered Chrome extension that brings **context-aware assistance, webpage summarization, and intelligent productivity workflows** directly into the browser.

## Overview

The **AI Productivity Browser Extension** is a Chrome Extension built with **Manifest V3** that uses **Google Gemini 2.5 Flash** to understand and interact with webpage content.

The extension extracts meaningful content from the active webpage, sends it to a Node.js backend for AI processing, and presents the generated results directly inside the browser extension interface.

The system combines **browser APIs, content extraction, LLM integration, conversational context, local storage, and a modular extension architecture** to create an AI-assisted browsing experience.

---

## ✨ Core Features

### 📝 AI-Powered Page Summarization

Automatically extracts the primary content from the active webpage and generates a concise AI summary using **Gemini 2.5 Flash**.

The content extraction pipeline removes common webpage noise such as:

- Navigation bars
- Headers
- Footers
- Sidebars
- Advertisements
- Scripts
- Styles

The system prioritizes semantic content containers such as:

```text
<article>
<main>
.content
#content
```

and falls back to the page body when required.

---

### 💬 Context-Aware Page Chat

Interact with an AI assistant about the webpage currently being viewed.

The chatbot receives:

- Page title
- Page description
- Relevant page content
- User query
- Conversation history

This enables multi-turn interaction where users can ask questions about the content without manually copying information from the webpage.

Example:

```text
"What is the main idea of this article?"
"Explain this in simple terms."
"What are the important points?"
```

---

### ⚡ Quick AI Actions

The extension provides predefined actions for common productivity workflows:

- 📝 Summarize the page
- 💡 Extract key points
- 🎯 Simplify the content

These actions automatically construct contextual prompts and send them to the AI backend.

---

### 🗂️ Summary History

Generated summaries are persisted locally using the **Chrome Storage API**.

The extension supports:

- Saving summaries
- Retrieving previous summaries
- Updating existing summaries
- Deleting individual summaries
- Clearing summary history
- Counting stored summaries

Summaries are associated with the source webpage URL and title.

---

### 📌 Summary Management

Users can interact with generated summaries through the extension UI.

Supported operations include:

- Pinning summaries
- Copying summaries to clipboard
- Clearing the current result
- Viewing summary history

---

### 🌙 Theme Support

The extension includes a light/dark theme switch with the selected preference persisted through browser local storage.

---

## 🏗️ System Architecture

```text
                    ┌─────────────────────────┐
                    │       Web Browser       │
                    │        Chrome           │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │   Chrome Extension      │
                    │       Manifest V3       │
                    └────────────┬────────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              │                  │                  │
              ▼                  ▼                  ▼
       ┌─────────────┐    ┌─────────────┐    ┌──────────────┐
       │    Popup    │    │   Content    │    │  Background  │
       │     UI      │    │   Scripts    │    │   Service    │
       └──────┬──────┘    └──────┬──────┘    │   Worker     │
              │                  │            └──────────────┘
              │                  ▼
              │          ┌───────────────┐
              │          │Content        │
              │          │Extractor      │
              │          └───────┬───────┘
              │                  │
              └──────────────────┤
                                 ▼
                    ┌─────────────────────────┐
                    │    Node.js Backend      │
                    │    Express Server       │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │   Gemini 2.5 Flash      │
                    │      Generative AI      │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │ AI Summary / AI Reply    │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │ Chrome Extension UI     │
                    └─────────────────────────┘
```

---

## 🔄 Processing Pipeline

### Page Summarization

```text
Active Webpage
      ↓
Chrome Tabs API
      ↓
Content Script Injection
      ↓
DOM Cloning
      ↓
Noise Element Removal
      ↓
Main Content Extraction
      ↓
Content Validation
      ↓
Node.js / Express API
      ↓
Gemini 2.5 Flash
      ↓
AI Generated Summary
      ↓
Chrome Storage
      ↓
Extension UI
```

---

## 🧠 Content Extraction Engine

The extension includes a dedicated content extraction utility.

### Extraction Strategy

The system first clones the webpage DOM to avoid modifying the original page.

It removes:

```text
script
style
nav
header
footer
aside
.ad
.advertisement
.sidebar
```

It then searches for meaningful content containers:

```text
article
main
.content
#content
```

If none are available, the extractor falls back to the complete document body.

The resulting text is normalized by collapsing unnecessary whitespace.

---

## 🤖 Generative AI Integration

The backend communicates with the **Google Gemini API** using REST requests.

The summarization endpoint sends webpage content to:

```text
Gemini 2.5 Flash
```

The summarization prompt requests:

```text
3–5 concise bullet points
```

The chatbot uses a separate contextual prompt containing:

```text
Page Title
Page Description
Page Content
Conversation History
Current User Query
```

This allows the AI assistant to generate responses grounded in the currently viewed webpage.

---

## 💬 Conversational Architecture

The page-chat workflow follows:

```text
User Query
    ↓
Active Tab Detection
    ↓
Page Context Extraction
    ↓
Conversation History
    ↓
Prompt Construction
    ↓
Gemini 2.5 Flash
    ↓
AI Response
    ↓
Chat Interface
```

Conversation history can be included in subsequent requests to support multi-turn interaction.

---

## 💾 Local Data Management

The extension uses:

```text
chrome.storage.local
```

through a dedicated storage abstraction:

```text
MyStorageManager
```

### Supported Operations

```text
saveSummary()
getAllSummaries()
getSummary()
deleteSummary()
clearAllSummaries()
getCount()
```

This separates storage logic from the popup UI and keeps the extension architecture modular.

---

## 🧩 Chrome Extension Architecture

The frontend is organized into independent extension components:

```text
Frontend/
│
├── background/
│   └── background.js
│
├── content/
│   ├── content.js
│   └── content.css
│
├── popup/
│   ├── popup.html
│   ├── popup.js
│   ├── popup.css
│   ├── history.html
│   ├── history.js
│   └── history.css
│
├── utils/
│   ├── contentExtractor.js
│   └── storage.js
│
├── icons/
│
└── manifest.json
```

---

## 📁 Project Structure

```text
AI-Productivity-Browser-Extension/
│
├── Frontend/
│   ├── background/
│   │   └── background.js
│   │
│   ├── content/
│   │   ├── content.js
│   │   └── content.css
│   │
│   ├── popup/
│   │   ├── popup.html
│   │   ├── popup.js
│   │   ├── popup.css
│   │   ├── history.html
│   │   ├── history.js
│   │   └── history.css
│   │
│   ├── utils/
│   │   ├── contentExtractor.js
│   │   └── storage.js
│   │
│   ├── icons/
│   └── manifest.json
│
├── backend/
│   ├── server.js
│   ├── index.js
│   ├── test-models.js
│   ├── package.json
│   └── package-lock.json
│
├── docs/
├── test.http
├── demo.txt
└── README.md
```

---

## 🔌 Backend API

The backend is implemented using **Node.js and Express**.

### Health Check

```http
GET /health
```

Example response:

```json
{
  "status": "ok",
  "message": "Backend running!"
}
```

### Summarization

```http
POST /api/summarize
```

Request:

```json
{
  "content": "Webpage content..."
}
```

Response:

```json
{
  "summary": "AI-generated summary..."
}
```

### Page Chat

```http
POST /chat
```

Request structure:

```json
{
  "message": "What is this article about?",
  "pageContext": {
    "title": "Page Title",
    "description": "Page Description",
    "mainContent": "Page Content"
  },
  "conversationHistory": []
}
```

Response:

```json
{
  "reply": "AI-generated response..."
}
```

---

## 🔐 Environment Configuration

The Gemini API key is kept on the backend and loaded through environment variables.

Create:

```text
.env
```

inside the backend directory:

```env
GEMINI_API_KEY=your_api_key_here
PORT=3000
```

> Never commit API keys or other secrets to source control.

---

## ⚙️ Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/jaggu1873/AI-Productivity-Browser-Extension.git
cd AI-Productivity-Browser-Extension
```

### 2. Install Backend Dependencies

```bash
cd backend
npm install
```

### 3. Configure Environment Variables

Create a `.env` file:

```env
GEMINI_API_KEY=your_api_key_here
PORT=3000
```

### 4. Start the Backend

```bash
node server.js
```

The backend runs by default at:

```text
http://localhost:3000
```

### 5. Load the Chrome Extension

Open:

```text
chrome://extensions/
```

Then:

1. Enable **Developer mode**
2. Select **Load unpacked**
3. Choose the `Frontend` directory
4. Pin the extension to the Chrome toolbar
5. Open a webpage
6. Launch the extension
7. Use **Summarize Page** or **Page Chat**

---

## 🛠️ Technology Stack

| Layer | Technologies |
|---|---|
| Extension Platform | Chrome Extension |
| Extension Standard | Manifest V3 |
| Frontend | HTML, CSS, JavaScript |
| Browser APIs | Chrome Tabs, Scripting, Storage |
| Content Processing | DOM / JavaScript |
| Backend | Node.js, Express |
| AI | Google Gemini 2.5 Flash |
| API Communication | REST / HTTP |
| Local Persistence | Chrome Storage API |
| Configuration | dotenv |
| Cross-Origin Support | CORS |

---

## 🎯 Engineering Highlights

- **Manifest V3 extension architecture**
- Dynamic content-script injection
- DOM-based webpage content extraction
- Noise filtering and semantic content selection
- REST-based LLM integration
- Context-aware AI prompting
- Multi-turn conversation handling
- Local browser persistence
- Modular frontend/backend separation
- API-key isolation on the backend
- Responsive extension UI
- Light/dark theme support
- Client-side summary management

---

## 🚀 Future Improvements

Potential extensions to the current architecture include:

- Improved webpage content extraction using semantic scoring
- Streaming AI responses
- More granular conversation persistence
- Additional AI-powered browser actions
- Productivity analytics
- Distraction detection and browsing-pattern analysis
- Configurable AI models/providers
- Production backend deployment
- Authentication and user-specific storage
- Improved privacy controls for webpage content

---

## 📌 Project Status

**Status:** Completed / Functional

The current implementation provides an operational Chrome extension with AI-powered webpage summarization, contextual page chat, quick AI actions, summary history, and browser-local storage.

---

## 👨‍💻 Author

**Jagadeesha V**

GitHub: [@jaggu1873](https://github.com/jaggu1873)

LinkedIn: [Jagadeesha V](https://www.linkedin.com/in/jagadeesha-v/)

---

> **AI inside the browser — turning webpages into actionable knowledge.**
