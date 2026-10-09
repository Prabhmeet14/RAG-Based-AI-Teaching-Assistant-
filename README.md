# RAG-Based AI Teaching Assistant 🎓

An AI-powered teaching assistant that uses **Retrieval-Augmented Generation (RAG)** to help users learn from educational videos. The system converts video lectures into searchable text, retrieves relevant information using vector embeddings, and generates context-aware answers using a Large Language Model (LLM).

## 🚀 Project Overview

Educational videos contain valuable knowledge, but finding a specific explanation in a long recording can be time-consuming. This project transforms video content into a searchable knowledge base, allowing users to ask questions in natural language and retrieve relevant information from their learning materials.

The system combines speech recognition, text processing, semantic search, and generative AI to create a practical question-answering pipeline.

**Key capabilities:**
- Convert educational videos into MP3 audio files.
- Transcribe Hindi speech and translate it into English text.
- Store transcript segments with timestamps and metadata.
- Generate vector embeddings for semantic search.
- Retrieve relevant content using cosine similarity.
- Generate context-aware answers using an LLM.

## 🛠️ Technologies, Libraries and Models

| Technology | Purpose |
|---|---|
| Python | Core programming language and pipeline orchestration |
| FFmpeg | Extracts audio from video files and converts it to MP3 |
| OpenAI Whisper (`large-v2`) | Transcribes speech and translates it into English |
| Ollama | Runs embedding and language models locally |
| BGE-M3 (`bge-m3`) | Generates embeddings for transcript chunks and user queries |
| Llama 3.2 (`llama3.2`) | Generates natural-language answers using retrieved context |
| Requests | Communicates with the local Ollama API |
| Pandas | Organizes transcript data and embeddings into a DataFrame |
| NumPy | Supports numerical data operations |
| Scikit-learn | Provides cosine similarity for semantic retrieval |
| Joblib | Saves and loads the processed embedding DataFrame |
| JSON | Stores structured transcript segments and metadata |

## 🏗️ Project Architecture

The project operates in two main phases.

**1. Knowledge-base preparation**

Educational videos → Audio extraction → Speech transcription and translation → JSON transcripts → Vector embeddings → Saved embedding DataFrame

**2. Question answering**

User query → Query embedding → Similarity search → Relevant transcript chunks → Context-aware prompt → LLM-generated answer

The embedding DataFrame is saved as `embeddings.joblib` and reused during question answering, avoiding the need to regenerate embeddings for every query.

## 🔄 Project Workflow

### 1. Video-to-Audio Conversion — `video_to_mp3.py`

Educational videos are collected in the `videos/` directory. The script uses FFmpeg to extract their audio and convert it into MP3 files stored in `audios/`.

### 2. Transcription and Translation — `mp3_to_json.py`

The MP3 files are processed using Whisper `large-v2`. The configured task translates Hindi speech into English text, which is saved as structured JSON transcript segments with timing information in the `jsons/` directory.

### 3. Embedding Generation — `preprocess_json.py`

The JSON transcripts are processed into a Pandas DataFrame. Ollama's `bge-m3` model generates vector embeddings for the transcript content, and Joblib saves the resulting DataFrame as `embeddings.joblib`.

### 4. Semantic Retrieval and Answer Generation — `process_incoming.py`

When a user submits a question, the script generates a query embedding and compares it with the stored transcript embeddings using cosine similarity. It retrieves the top five relevant chunks and constructs a prompt containing the question and supporting transcript context.

The prompt is sent to Ollama's `llama3.2` model, which generates a response. The current implementation saves the prompt in `prompt.txt` and the answer in `response.txt`.

## 📂 Project Structure

```text
RAG-Based-AI-Teaching-Assistant/
│
├── videos/                 # Source educational videos
├── audios/                 # Converted MP3 audio files
├── jsons/                  # Timestamped transcript JSON files
│
├── video_to_mp3.py         # Video-to-audio conversion
├── mp3_to_json.py          # Transcription and translation
├── preprocess_json.py      # Transcript processing and embeddings
├── process_incoming.py     # Retrieval and answer generation
│
├── requirements.txt        # Python dependencies
└── .gitignore              # Files excluded from Git tracking
```

Generated artifacts such as `embeddings.joblib`, audio files, transcripts, prompts, and responses are excluded from the GitHub repository and are created locally during processing.

## 📖 How to Use the Project with Your Own Data

Follow this workflow to build a searchable knowledge base from your own educational videos.

**Step 1 — Collect your videos**

Place your educational video files inside the `videos/` folder.

**Step 2 — Convert videos to MP3**

```bash
python video_to_mp3.py
```

**Step 3 — Generate English transcripts**

```bash
python mp3_to_json.py
```

This creates transcript JSON files in `jsons/`. The current configuration is intended for Hindi-language audio translated into English.

**Step 4 — Generate vector embeddings**

```bash
python preprocess_json.py
```

This processes the transcripts, generates embeddings using `bge-m3` through Ollama, and saves the DataFrame as `embeddings.joblib`.

**Step 5 — Ask questions about your content**

```bash
python process_incoming.py
```

The script loads the saved embeddings, retrieves relevant transcript chunks, constructs a prompt, and generates an answer using `llama3.2`.

Before running the workflow, ensure Python dependencies, FFmpeg, and Ollama are installed, and that the required Ollama models are available locally. Check `process_incoming.py` for its specific method of receiving the user's question.

When adding new videos, rerun the transcription and embedding stages so the knowledge base includes the new content.

## 💡 Example Use Case

Imagine a student has several programming tutorials and wants to understand a specific concept.

The student asks:

> What is the difference between a Python list and a tuple?

The system searches the transcript embeddings for relevant passages, retrieves the most relevant chunks, and provides them to the LLM as context for generating an answer. Transcript timestamps can also help the student locate the relevant section of the original video.

This illustrates the intended workflow; actual answer quality depends on the available transcripts and retrieval results.

## ⚠️ Limitations

- Transcription quality depends on audio clarity, background noise, accents, and language settings.
- Answers depend on the relevance and completeness of retrieved transcript chunks.
- LLM-generated responses may contain inaccuracies and should be checked against the original learning material.
- Whisper `large-v2` and local language models can require significant memory and processing resources.
- The current implementation uses a fixed top-five retrieval setting and has not been formally evaluated.
- The workflow uses Python scripts and text files rather than a dedicated interactive chat interface.

## 🔮 Future Improvements

- Build a web-based chat interface using Streamlit or FastAPI.
- Add interactive video timestamps and source references.
- Support multiple source languages and configurable translation.
- Introduce a vector database for larger knowledge bases.
- Improve retrieval with metadata filtering and reranking.
- Add automated testing, evaluation metrics, and stronger error handling.
- Support multiple courses and independent knowledge bases.

## 🎯 Skills Demonstrated

**AI Engineering:** Retrieval-Augmented Generation (RAG), LLM integration, prompt engineering, local model inference.

**Machine Learning and NLP:** Speech recognition, translation, vector embeddings, semantic search, cosine similarity.

**Data Engineering:** Data preprocessing, JSON handling, pipeline orchestration, API integration, and serialized data storage.


GitHub: [Prabhmeet14](https://github.com/Prabhmeet14)
