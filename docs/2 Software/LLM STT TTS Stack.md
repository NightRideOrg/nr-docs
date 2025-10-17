**LLM Orchestrator**
- Local: ~8B LLM w/ GPU acceleration (TensorRT-LLM) for simple intents.

**STT**
- Whisper (TensorRT) or Vosk (CPU-friendly). Target <300 ms latency short utterances.

**TTS**
- Piper (lightweight, offline) or NVIDIA Riva TTS on Orin if you want quality.

**Tool calls**
- Navigation: `navigate_to_address`, `navigate_to_coordinates`, `show_coordinates`
- Media: `get_current_title/artist`, `pause_song`, `resume_song`

**Prompting style**
- Short system prompt: “Drive-safe assistant. Confirm actions. Never control vehicle.”
- Inject CAN context (speed, gear) for safety-aware replies.
