```mermaid
flowchart TD
  %% Set direction top to bottom
  %% Input Layer
  subgraph Input["Input Layer"]
    A[Voice Input - Microphone]
    B[Text Input - Textbox or File]
  end

  %% Processing Layer
  subgraph Processing["Processing Layer"]
    C[STT Module - Whisper or Vosk]
    D[Agent Controller - Routing and Optional Features]
    E[TTS Module - gTTS or Coqui]
  end

  %% Output Layer
  subgraph Output["Output Layer"]
    F[Display Output - Text Transcript]
    G[Voice Output - Audio Playback]
  end

  %% UI Layer
  subgraph UI["User Interface"]
    H[Streamlit / Gradio]
  end

  %% Connections
  A --> C
  C --> D
  B --> D
  D --> F
  D --> E
  E --> G
  F --> H
  G --> H
  B --> H
