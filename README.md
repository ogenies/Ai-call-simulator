# Simulateur d'appels sortants IA

A production-ready Streamlit application for practicing outbound sales calls in French. The trainee plays the call center agent, and an Ollama-powered `qwen2.5:3b` model plays only the customer prospect.

The objection knowledge base is derived from `Objections commerciales (1).pdf`. The AI uses those objections and reaction patterns as private behavioral guidance, then speaks naturally to the agent as the prospect.

## Features

- Customer profiles: busy, aggressive, suspicious, curious, and well-informed.
- Difficulty levels: beginner, intermediate, and advanced.
- AI customer speaks first and stays in character.
- Conversation memory persists throughout the call.
- PDF-derived, knowledge-base-guided objections from `data/knowledge_base.json`.
- French Streamlit interface.
- Agent voice input with `st.audio_input`.
- French speech-to-text through Faster-Whisper.
- Browser-based French speech synthesis for the AI prospect response.
- Real call recordings can be uploaded and transcribed as style references.
- Phone-call-style prospect replies: no written answer format, no coaching, no labels.
- Automatic evaluation across seven sales skills with a final score out of 100.
- Modular Python 3.12 code with separated engine and UI layers.

## Requirements

- Python 3.12
- Ollama running locally
- `qwen2.5:3b` pulled in Ollama
- Microphone access in the browser
- Network access the first time Faster-Whisper downloads its speech model

## Setup

```bash
cd AI_Call_Simulator
python3.12 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
ollama pull qwen2.5:3b
streamlit run app.py
```

If Ollama is already running on another host, update `OllamaClient` usage in `engine/conversation.py` or pass a different URL through `ConversationEngine.from_paths`.

## Project Structure

```text
AI_Call_Simulator/
  app.py
  engine/
    conversation.py
    customer_state.py
    evaluator.py
    knowledge.py
    llm.py
    prompt_builder.py
  ui/
    streamlit_app.py
  data/
    knowledge_base.json
    profiles.json
  requirements.txt
  README.md
```

## Notes

## Voice Workflow

1. Click `Démarrer l'appel`.
2. The AI prospect speaks first and the browser reads the line aloud in French.
3. Record your answer with the microphone.
4. Click `Transcrire l'audio`.
5. Review or edit the transcript.
6. Click `Envoyer au prospect`.
7. The AI prospect reacts and speaks again.

The app also accepts real call recordings in the sidebar. Those recordings are transcribed and passed to the prompt as style references so the AI prospect can imitate the rhythm and tone of real calls without becoming scripted.

Piper TTS is not required in this version because browser speech synthesis provides immediate spoken French output. The engine remains modular enough to replace it with Piper later.
