# RAG-Based AI Teaching Assistant 🎓

## Project Overview

The **RAG-Based AI Teaching Assistant** is a local AI-powered question-answering system that helps users learn from educational video content. It combines **Retrieval-Augmented Generation (RAG)**, speech transcription, vector embeddings, semantic similarity search, and a large language model (LLM) to generate answers grounded in the content of source videos.

The project uses three educational videos as its source material. The videos are converted into audio files, transcribed from Hindi into English, and processed into searchable text chunks. These chunks are converted into vector embeddings and retrieved when a user submits a question.

Instead of relying only on the language model's general knowledge, the system supplies relevant transcript excerpts as context to help generate answers related to the original educational material.

The entire pipeline runs locally using Python, FFmpeg, Whisper, and Ollama-supported models.

## Technologies, Libraries and Models

| Technology | Purpose |
|---|---|
| Python | Core programming language for the complete pipeline |
| FFmpeg | Converts source videos into MP3 audio files |
| OpenAI Whisper (`large-v2`) | Transcribes Hindi audio into English text |
| Ollama | Runs embedding and language models locally |
| `bge-m3` | Generates vector embeddings for transcripts and user questions |
| `llama3.2` | Generates answers using the retrieved context |
| Requests | Communicates with the local Ollama API |
| Pandas | Organizes transcript chunks and embedding data |
| NumPy | Supports numerical operations on embedding vectors |
| Scikit-learn | Provides cosine similarity for semantic retrieval |
| Joblib | Saves and loads the generated embeddings |
| JSON | Stores timestamped transcript data |

## Project Architecture

The project follows a Retrieval-Augmented Generation architecture consisting of two main stages.

### 1. Data Processing and Indexing

The source videos are converted into audio, transcribed into English, and transformed into searchable vector representations. The resulting embeddings are stored locally for later retrieval.

### 2. Question Answering and Retrieval

When a user submits a question, the system generates an embedding for the query, identifies the most semantically similar transcript chunks, and passes the retrieved context to the language model to generate a response.

This separation allows transcript processing to be performed before questions are asked, avoiding the need to regenerate all embeddings for every query.

## Project Workflow

### Step 1: Video-to-Audio Conversion

**Script:** `video_to_mp3.py`

The project begins with three educational videos stored in the `videos/` directory.

The script uses FFmpeg to extract the audio from each video and convert it into an MP3 file. The resulting audio files are stored in the `audios/` directory.

**Input:** Three source video files in `videos/`

**Output:** Converted MP3 audio files in `audios/`

### Step 2: Audio Transcription

**Script:** `mp3_to_json.py`

The audio files are processed using OpenAI Whisper's `large-v2` model. The
