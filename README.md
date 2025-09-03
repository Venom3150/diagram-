```mermaid
flowchart TD
  %% Input layer
  A[Voice Input (Microphone)] --> B[STT Module (Whisper / Vosk)]
  C[Text Input (Textbox / File)] --> D[Agent Controller (Routing & Optional Features)]
  B --> D

  %% Output layer
  D --> E[Display Output (Text Transcript)]
  D --> F[TTS Module (gTTS / Coqui)]
  F --> G[Voice Output (Audio Playback)]

  %% UI connections (dashed)
  H[User Interface (Streamlit / Gradio)]
  H --- A
  H --- C
  E --- H
  G --- H
