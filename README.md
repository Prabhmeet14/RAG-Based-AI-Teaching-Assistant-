# RAG-Based AI Teaching Assistant 🎓

An AI-powered teaching assistant that uses **Retrieval-Augmented Generation (RAG)** to help users learn from educational videos. The system converts video lectures into searchable text, retrieves relevant information using vector embeddings, and generates context-aware answers using a Large Language Model (LLM).

## 🚀 Project Overview

Educational videos contain valuable knowledge, but finding a specific explanation in a long recording can be time-consuming. This project transforms video content into a searchable knowledge base, allowing users to ask questions in natural language and retrieve relevant information from their learning materials.

The project uses **three educational videos** as its initial source material. The videos, converted MP3 audio files, JSON transcripts, and preprocessed embeddings are intended to be included in the GitHub repository to demonstrate the complete knowledge-processing pipeline.

The system combines speech recognition, text processing, semantic search, and generative AI to create a question-answering pipeline that runs locally using Python and Ollama.

**Key capabilities:**

- Process three educational videos as the initial source material.
- Convert videos into MP3 audio files.
- Transcribe Hindi speech and translate it into English text.
- Store transcript segments with timestamps and metadata in JSON files.
- Generate vector embeddings for semantic search.
- Retrieve relevant content using cosine similarity.
- Generate context-aware answers using an LLM.
- Save the processed embedding DataFrame for reuse during question answering.

## 🛠️ Technologies, Libraries and Models

| Technology | Purpose |
|---|---|
| Python | Core programming language and pipeline orchestration |
| FFmpeg | Extracts audio from video files and converts it to MP3 |
| OpenAI Whisper (`large-v2`) | Translates Hindi speech into English text |
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

Three educational videos → Audio extraction → Speech transcription and translation → JSON transcripts → Vector embeddings → Saved embedding DataFrame

**2. Question answering**

User query → Query embedding → Similarity search → Five relevant transcript chunks → Context-aware prompt → LLM-generated answer

The embedding DataFrame is saved as `embeddings.joblib` and reused during question answering, avoiding the need to regenerate embeddings for every query.

## 🔄 Project Workflow

### 1. Video-to-Audio Conversion — `video_to_mp3.py`

Three educational videos are stored in the `videos/` directory. The script uses FFmpeg to extract their audio and convert each video into an MP3 file.

The generated audio files are saved in the `audios/` directory.

- **Input:** Three source video files in `videos/`
- **Output:** Converted MP3 files in `audios/`

### 2. Transcription and Translation — `mp3_to_json.py`

The MP3 files are processed using Whisper `large-v2`. The script is configured with Hindi as the source language and the translation task to produce English text.

The resulting transcripts are saved as structured JSON files in the `jsons/` directory. Transcript segments retain timing information so retrieved content can be associated with its position in the source material.

- **Input:** MP3 files from `audios/`
- **Output:** English transcript JSON files in `jsons/`

### 3. Embedding Generation — `preprocess_json.py`

The JSON transcripts are processed into a Pandas DataFrame. The script communicates with Ollama's local API to generate vector embeddings using the `bge-m3` model.

These embeddings represent the semantic meaning of the transcript content, allowing the system to identify relevant passages based on meaning rather than exact keyword matches.

Joblib saves the processed DataFrame and embeddings as `embeddings.joblib`.

- **Input:** Transcript JSON files from `jsons/`
- **Output:** `embeddings.joblib`

### 4. Semantic Retrieval and Answer Generation — `process_incoming.py`

When a user submits a question, the script generates a query embedding using `bge-m3` and compares it with the stored transcript embeddings using cosine similarity.

The system retrieves the **top five most relevant transcript chunks** and constructs a prompt containing the user's question, supporting transcript text, and associated metadata where available.

The prompt is sent to Ollama's `llama3.2` model, which generates a context-aware response. The current implementation saves the prompt in `prompt.txt` and the generated answer in `response.txt`.

- **Input:** User question and saved transcript embeddings
- **Output:** Retrieved context, `prompt.txt`, and `response.txt`

## 📂 Project Structure

```text
RAG-Based-AI-Teaching-Assistant/
│
├── videos/                 # Three source educational videos
├── audios/                 # MP3 files converted from the videos
├── jsons/                  # JSON transcripts generated by mp3_to_json.py
├── embeddings.joblib       # Saved transcript embeddings and metadata
│
├── video_to_mp3.py         # Video-to-audio conversion
├── mp3_to_json.py          # Transcription and translation
├── preprocess_json.py      # Transcript processing and embedding generation
├── process_incoming.py     # Semantic retrieval and answer generation
│
├── requirements.txt        # Python dependencies
└── .gitignore              # Files excluded from Git tracking
```

The repository is intended to include the three source videos, their converted MP3 audio files, the JSON transcripts, and `embeddings.joblib`, alongside the Python scripts and dependency file.

The `videos/`, `audios/`, and `jsons/` directories contain the data used at different stages of the pipeline. The `embeddings.joblib` file contains the processed embedding data generated by `preprocess_json.py` and loaded by `process_incoming.py`.

The scripts can regenerate intermediate outputs when needed, while the saved embeddings allow the retrieval stage to reuse previously processed transcript data.

## 📖 How to Use the Project with Your Own Data

Follow this workflow to build a searchable knowledge base from your own educational videos.

### Step 1 — Collect your videos

Place your educational video files inside the `videos/` folder. The current project uses three source videos, but the pipeline can be adapted for additional videos.

### Step 2 — Convert videos to MP3

```bash
python video_to_mp3.py
```

This extracts audio from the source videos and saves the resulting MP3 files in `audios/`.

### Step 3 — Generate English transcripts

```bash
python mp3_to_json.py
```

This processes the audio files and creates transcript JSON files in `jsons/`. The current configuration is intended for Hindi-language audio translated into English.

### Step 4 — Generate vector embeddings

```bash
python preprocess_json.py
```

This processes the transcripts, generates embeddings using `bge-m3` through Ollama, and saves the processed DataFrame as `embeddings.joblib`.

### Step 5 — Ask questions about your content

```bash
python process_incoming.py
```

The script loads the saved embeddings, retrieves relevant transcript chunks, constructs a prompt, and generates an answer using `llama3.2`.

Before running the workflow, ensure that the Python dependencies, FFmpeg, and Ollama are installed and that the required Ollama models are available locally. Check `process_incoming.py` for its specific method of receiving the user's question.

When adding new videos, rerun the transcription and embedding stages so the knowledge base includes the new content. The scripts should be run in the correct order because each stage depends on outputs from the preceding stage.

If you use the preprocessed files already included in the repository, ensure that `embeddings.joblib` matches the transcript data and embedding format expected by `process_incoming.py`.

## 💡 Example Use Case

Imagine a student has three programming tutorials and wants to understand a specific concept without searching through every video manually.

The student asks:

> What is the difference between a Python list and a tuple?

The system searches the transcript embeddings for relevant passages, retrieves the most relevant chunks, and provides them to the LLM as context for generating an answer.

Transcript timestamps can also help the student locate the relevant section of the original video. The quality of the answer depends on the available transcripts and the relevance of the retrieved content.

## ⚠️ Limitations

- Transcription quality depends on audio clarity, background noise, accents, and language settings.
- Answers depend on the relevance and completeness of the retrieved transcript chunks.
- LLM-generated responses may contain inaccuracies and should be checked against the original learning material.
- Whisper `large-v2` and local language models can require significant memory and processing resources.
- The current implementation uses a fixed top-five retrieval setting and has not been formally evaluated.
- The workflow uses Python scripts and text files rather than a dedicated interactive chat interface.
- The system is designed to retrieve information from processed video content, but the LLM may sometimes generate information beyond the retrieved context.
- The saved embeddings must remain compatible with the embedding model and data structure expected by the retrieval script.

## 🔮 Future Improvements

- Build a web-based chat interface using Streamlit or FastAPI.
- Display interactive video timestamps and source references alongside answers.
- Support multiple source languages and configurable translation.
- Introduce a vector database for larger knowledge bases.
- Improve retrieval with metadata filtering, better chunking strategies, and reranking.
- Add automated testing, evaluation metrics, and stronger error handling.
- Support multiple courses and independent knowledge bases.
- Add conversation history to support follow-up questions.

## 🎯 Skills Demonstrated

**AI Engineering:** Retrieval-Augmented Generation (RAG), LLM integration, prompt engineering, and local model inference.

**Machine Learning and NLP:** Speech recognition, translation, vector embeddings, semantic search, and cosine similarity.

**Data Engineering:** Data preprocessing, JSON handling, pipeline orchestration, API integration, and serialized data storage.

**Software Development:** Python scripting, modular project design, dependency management, Git, and GitHub documentation.
