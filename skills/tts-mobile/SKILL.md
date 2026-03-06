---
name: tts-mobile
description: "Mobile-optimized Text-to-Speech skill. Converts agent responses or document summaries into high-quality audio files (MP3/OGG) for listening on the go. Integrated with ElevenLabs and local ONNX-based TTS engines."
---

# tts-mobile

Listen to your agent's insights while commuting or working out.

## Features
- **Voice Selection**: Choose from 10+ premium voices via ElevenLabs.
- **Local Fallback**: Uses `sherpa-onnx` for offline speech generation when data is limited.
- **Audio Briefings**: Automatically converts the "Monday Briefing" into an audio message.
- **WhatsApp Integration**: Sends audio files directly to your WhatsApp as voice messages.

## Usage
`@OpenClaw read-this "Summarize the latest AI news and send as audio"`
`@OpenClaw set-voice "Serena"`
