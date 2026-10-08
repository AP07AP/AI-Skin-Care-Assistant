# AI Skin Care Assistant

A voice and image skin-care consultation app. Describe your skin concern by voice, upload a photo, and get a short doctor-style answer as both text and audio.

> **Disclaimer:** This project gives general information only. It is not a medical diagnosis and does not replace a licensed dermatologist or doctor.

## How It Works

1. **Speak:** Record or upload a voice description of your skin concern.
2. **Transcribe:** Groq's Whisper model (`whisper-large-v3`) converts the audio to text.
3. **Analyze:** The text and your skin image go to a Groq vision model (Llama 4 Scout), which writes a short, plain-language reply.
4. **Listen:** Deepgram text-to-speech reads the reply aloud in a doctor-style voice.

The interface is built with Gradio and shows the transcript, the written reply, and the audio player.

## Tech Stack

Python, Gradio, Groq API (Whisper + Llama 4 Scout vision), Deepgram TTS, Pillow, pydub, uv

## Project Structure

```
├── main.py                      # Gradio app and main pipeline
├── voice_of_the_patient.py      # Audio recording and Groq transcription
├── brain_of_the_doctor_groq.py  # Groq vision model reply (used by main.py)
├── brain_of_the_doctor.py       # Alternative MiniMax implementation (not used)
├── voice_of_the_doctor.py       # Deepgram text-to-speech
├── free_text_to_speech.py       # Free gTTS alternative for voice output
├── sample.env                   # Template for API keys
└── pyproject.toml               # Dependencies
```

## Requirements

- Python 3.11 or newer
- [uv](https://docs.astral.sh/uv/) package manager
- FFmpeg and PortAudio (needed for audio handling)
- Free API keys from [Groq](https://console.groq.com) and [Deepgram](https://deepgram.com)

## Setup

1. Install FFmpeg and PortAudio for your system.
   - Windows: `choco install ffmpeg portaudio`
   - macOS: `brew install ffmpeg portaudio`
   - Ubuntu/Debian: `sudo apt install ffmpeg portaudio19-dev python3-dev build-essential`
2. Install dependencies:
   ```
   uv sync
   ```
3. Copy `sample.env` to `.env` and add your keys:
   ```
   GROQ_API_KEY=your_groq_key
   DEEPGRAM_API_KEY=your_deepgram_key
   ```

## Run

```
uv run python main.py
```

Open http://127.0.0.1:7860 in your browser, record or upload your voice, add a skin image, and click Analyze.

## Optional Settings

You can override the models in `.env`:

```
WHISPER_MODEL=whisper-large-v3
GROQ_MODEL=meta-llama/llama-4-scout-17b-16e-instruct
DEEPGRAM_TTS_MODEL=aura-2-thalia-en
```

## Known Limitations

- The Groq vision model only accepts images, so an uploaded video is not analyzed. The app uses the uploaded image instead.
- Replies are short, general guidance and can be wrong. Always see a dermatologist for real concerns.
- Free API tiers are rate-limited and have no privacy guarantees, so do not upload sensitive personal photos while testing.

