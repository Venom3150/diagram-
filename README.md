```mermaid
%%{init: {'theme': 'default'}}%%
flowchart TD
  %% Input layer
  A[Voice Input (Microphone)]:::input --> B[STT Module (Whisper / Vosk)]:::proc
  C[Text Input (Textbox / File)]:::input --> D[Agent Controller (Routing & Optional Features)]:::proc
  B --> D

  %% Output layer
  D --> E[Display Output (Text Transcript)]:::output
  D --> F[TTS Module (gTTS / Coqui)]:::output
  F --> G[Voice Output (Audio Playback)]:::output

  %% UI connections (dashed)
  H[User Interface (Streamlit / Gradio)]:::ui
  H --- A
  H --- C
  E --- H
  G --- H

  %% Styling
  classDef input fill:#add8e6,stroke:#333,stroke-width:1px,color:#000;
  classDef proc  fill:#f9f9a6,stroke:#333,stroke-width:1px,color:#000;
  classDef output fill:#f8cfe0,stroke:#333,stroke-width:1px,color:#000;
  classDef ui    fill:#d3d3d3,stroke:#333,stroke-width:1px,color:#000;
