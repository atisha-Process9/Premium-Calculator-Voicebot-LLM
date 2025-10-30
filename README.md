# Voice-Live-Api
Voice Live API Demo — a simple web frontend that lets you talk to Azure OpenAI Voice Live (Realtime) over WebSocket. It captures your microphone audio, streams it to Azure, and plays back the model's real‑time speech while also showing transcripts.

## What this project does
- **Real‑time voice chat:** Stream microphone audio to Azure OpenAI Voice Live and hear AI responses back with minimal latency.
- **Text + audio modalities:** Sends audio, receives audio, and shows text transcripts for both user and AI.
- **Configurable model and voice:** Choose Azure OpenAI model (e.g., `gpt-4o-*`, `phi4-*`) and Azure Speech voice.
- **Barge‑in/interruption:** If you start speaking while the AI is talking, it cancels the current response and listens to you.

## Quick start
1) Create and activate a Python virtual environment
```bash
python3 -m venv venv
source venv/bin/activate
```

2) Install dependencies
```bash
pip install -r requirements.txt
```

3) Run the local web server
```bash
python3 serve.py
```
This serves the frontend at `http://localhost:8060/` and automatically opens your browser. The server adds the headers required for AudioWorklets.

4) Configure in the UI
- **Endpoint:** Your Azure OpenAI endpoint, e.g. `https://your-resource.cognitiveservices.azure.com/`
- **API Key:** A valid Azure OpenAI API key for the resource above
- **Model:** Pick a supported model (e.g., `gpt-4o-realtime-preview`, `gpt-4o-mini-realtime-preview`, `phi4-mm-realtime`, etc.)
- **Voice:** Select an Azure Speech voice

5) Click “Start Chat” and speak
- You’ll see user/AI transcripts in the chat area.
- Click “Stop Chat” (or press Escape) to end the session.

## Files overview
- `index.html`: The UI for entering endpoint, API key, selecting model/voice, and viewing chat.
- `voice-live-client.js`: Main client logic. Handles microphone capture, AudioWorklet wiring, WebSocket connection to Azure (`/voice-live/realtime`), session configuration, streaming audio up and down, transcripts, and barge‑in.
- `audio-processor.js`: AudioWorkletProcessor that buffers 24kHz mono audio and sends 16‑bit PCM chunks to the main thread for upload.
- `serve.py`: A small HTTP server with proper headers for running AudioWorklets locally and opening your browser at `http://localhost:8060`.
- `requirements.txt`: Python dependencies for the local server and helper libs.

## Configuration details
- The WebSocket URL is built as:
  `wss://<your-endpoint-host>/voice-live/realtime?api-version=2025-05-01-preview&model=<MODEL>&api-key=<API_KEY>`
- The session update enables both `audio` and `text` modalities and requests input transcription. Turn‑detection uses `azure_semantic_vad`.
- Sample rate is 24kHz mono. Audio is streamed as base64‑encoded 16‑bit PCM chunks.


## Troubleshooting
- "Microphone permission denied": Allow mic access in the browser.
- No audio output: Ensure system output is not muted and the AudioContext isn’t suspended (user gesture usually required; the Start button handles this).
- CORS/Worklet errors: Always launch with `python3 serve.py` so the correct headers are set.
- Connection errors: Verify endpoint URL, API key, and that the selected model is enabled on your Azure resource and region.

## Resources
- **Step‑by‑step video:** [YouTube walkthrough](https://www.youtube.com/watch?v=F4h-gQ1s4SY)
- **Official docs:** [Azure OpenAI Realtime (Voice Live) over WebSocket](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/voice-live-quickstart?tabs=linux%2Ckeyless&pivots=programming-language-python)
- **Azure Speech voice list:** [Supported languages and voices](https://learn.microsoft.com/en-us/azure/ai-services/speech-servicevoice-live-language-support?tabs=speechinput)

## License
This project is provided as a demo. Review your organization’s policies and Azure terms when using Azure OpenAI.
