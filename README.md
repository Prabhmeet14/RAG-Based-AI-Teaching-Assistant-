@'
# RAG-Based AI Teaching Assistant

A Retrieval-Augmented Generation (RAG) based AI teaching assistant using Python, embeddings, and language models to help users learn from educational content.

## Overview

This project processes educational videos, converts their audio into English transcripts, generates embeddings, and retrieves relevant content to answer user questions using a large language model.

## Technologies Used

- Python
- OpenAI Whisper (`large-v2`) for transcription and translation
- FFmpeg for video-to-audio conversion
- Ollama (`bge-m3`) for embeddings
- Ollama (`llama3.2`) for response generation
- Pandas and Joblib for data handling
- Scikit-learn for cosine similarity

## Workflow

1. Convert educational videos into MP3 audio files.
2. Transcribe and translate the audio into English text.
3. Save the transcript chunks as JSON files with timestamps.
4. Generate and save embeddings for the transcript chunks.
5. Retrieve relevant transcript chunks based on the user's query.
6. Build a context-aware prompt and generate an answer using the language model.

## Prerequisites

- Python installed
- FFmpeg installed and available on PATH
- Ollama installed and running
- The `bge-m3` and `llama3.2` models downloaded in Ollama

## Installation

Install the Python dependencies:

```bash
pip install -r requirements.txt
