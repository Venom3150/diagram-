```mermaid
flowchart TD
  %% Input layer
  A[Voice Input - Microphone] --> B[STT Module - Whisper or Vosk]
  C[Text Input - Textbox or File] --> D[Agent Controller - Routing and Optional Features]
  B --> D

  %% Output layer
  D --> E[Display Output - Text Transcript]
  D --> F[TTS Module - gTTS or Coqui]
  F --> G[Voice Output - Audio Playback]

  %% UI connections
  H[User Interface - Streamlit or Gradio]
  H --- A
  H --- C
  E --- H
  G --- H
