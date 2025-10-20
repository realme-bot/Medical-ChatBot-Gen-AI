
# 🏥 Medical Chatbot - RAG System with Pinecone

An intelligent medical question-answering chatbot built using **Retrieval-Augmented Generation (RAG)**, **Sentence Transformers**, and **Pinecone Vector Database**. The chatbot processes medical textbooks and provides accurate, context-aware answers to medical queries in under 1 second.

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Pinecone](https://img.shields.io/badge/Pinecone-Vector_DB-green.svg)](https://www.pinecone.io/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## 📋 Table of Contents

- [Features](#-features)
- [Demo](#-demo)
- [Architecture](#-architecture)
- [Installation](#-installation)
- [Usage](#-usage)
- [Project Structure](#-project-structure)
- [Configuration](#-configuration)
- [Performance](#-performance)
- [Examples](#-examples)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [License](#-license)

## ✨ Features

- 🚀 **Lightning-fast queries** - Sub-second response time after initialization
- 🎯 **Smart relevance filtering** - Rejects non-medical questions automatically
- 📚 **Context-aware answers** - Retrieves multiple relevant chunks from medical literature
- 🔧 **Optimized chunking** - Intelligent semantic text splitting (200-350 words)
- ⚡ **Singleton pattern** - Model loads once, queries run instantly
- 📊 **Relevance scoring** - Shows confidence scores for each answer
- 🧠 **Sentence transformers** - Uses all-MiniLM-L6-v2 for embeddings (384 dimensions)
- 💾 **Vector database** - Pinecone for scalable semantic search

## 🎬 Demo

```python
from fast_query_system import ask, query

# Simple query
print(ask("What is plasma?"))
# ⚡ Answered in 0.456s
# Output: "Plasma is the liquid component of blood..."

# Detailed query with sources
print(query("What is anemia?", top_k=3))
# Shows multiple relevant chunks with relevance scores

# Non-medical query (auto-filtered)
print(ask("Who is the president?"))
# ❌ I don't know about this. Please ask medical questions...
```

## 🏗️ Architecture

```
┌─────────────────┐
│  Medical PDF    │
│   Textbook      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ PDF Processing  │  ← PyMuPDF extraction
│  & Chunking     │  ← Semantic chunking (200-350 words)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   Embeddings    │  ← SentenceTransformer (all-MiniLM-L6-v2)
│   Generation    │  ← 384-dimensional vectors
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Pinecone DB    │  ← Vector storage
│   (Serverless)  │  ← Cosine similarity search
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Query System   │  ← User question
│  (RAG-based)    │  ← Retrieves top-k chunks
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Smart Filtering │  ← Relevance threshold (0.35)
│  & Response     │  ← Formatted answer
└─────────────────┘
```

## 📦 Installation

### Prerequisites

- Python 3.8 or higher
- Pinecone API key
- PDF medical textbook

### Step 1: Clone the Repository

```bash
git clone https://github.com/realme-bot/Medical-ChatBot-Gen-AI.git
cd medical-chatbot
```

### Step 2: Install Dependencies

```bash
pip install -r requirements.txt
```

**requirements.txt:**
```txt
sentence-transformers==2.2.2
pinecone-client==3.0.0
python-dotenv==1.0.0
pymupdf==1.23.0
tqdm==4.66.0
```

### Step 3: Set Up Environment Variables

Create a `.env` file in the project root:

```env
PINECONE_API_KEY=your_pinecone_api_key_here
PINECONE_ENV=us-east-1
```

Get your Pinecone API key from: [https://www.pinecone.io/](https://www.pinecone.io/)

## 🚀 Usage

### Step 1: Process Your Medical Textbook

```python
# Edit the PDF path in optimized_pipeline.py
PDF_PATH = r"path/to/your/medical_textbook.pdf"

# Run the pipeline (one-time setup)
python optimized_pipeline.py
```

**Expected output:**
```
🏥 OPTIMIZED MEDICAL CHATBOT - PDF PROCESSING
============================================================
📖 Opening PDF...
Extracting pages: 100%|██████████| 1200/1200
✅ Extracted 1200 pages
🧹 Cleaning text...
✅ Cleaned text length: 2,500,000 characters
✂️ Using hybrid semantic chunking...
✅ Created 8,500 chunks
📏 Chunk statistics:
   - Average: 215 words
   - Min: 52 words
   - Max: 348 words
🧠 Loading embedding model: all-MiniLM-L6-v2...
🔢 Generating embeddings for 8,500 chunks...
✅ Generated 8,500 embeddings (dim: 384)
📤 Uploading 8,500 vectors to Pinecone...
✅ All vectors uploaded!
📊 Index stats: 8,500 total vectors
🚀 Ready for high-quality queries!
```

**Time:** 5-10 minutes (one-time only)

### Step 2: Query the Chatbot

#### In Python Script:

```python
from fast_query_system import ask, query

# Simple query
answer = ask("What is hemoglobin?")
print(answer)

# Detailed query with multiple sources
answer = query("Explain the cardiovascular system", top_k=5)
print(answer)
```

#### In Jupyter Notebook:

```python
# Cell 1: Import
from fast_query_system import ask, query

# Cell 2: Ask questions
print(ask("What is plasma?"))
print(ask("What are red blood cells?"))
print(ask("Explain anemia"))

# Cell 3: Detailed query
print(query("What is hemostasis?", top_k=3))
```

#### Batch Processing:

```python
from fast_query_system import batch_ask

questions = [
    "What is plasma?",
    "What are platelets?",
    "Define hemoglobin",
    "What is leukemia?"
]

results = batch_ask(questions)
```

## 📁 Project Structure

```
medical-chatbot/
│
├── optimized_pipeline.py      # PDF processing & upload to Pinecone
├── fast_query_system.py       # Fast query interface
├── requirements.txt           # Python dependencies
├── .env                       # Environment variables (API keys)
├── .gitignore                 # Git ignore file
├── README.md                  # This file
│
├── data/                      # Medical textbooks (PDF)
│   └── medical_textbook.pdf
│
└── notebooks/                 # Jupyter notebooks (optional)
    └── demo.ipynb
```

## ⚙️ Configuration

### Chunking Parameters

Edit in `optimized_pipeline.py`:

```python
CHUNK_SIZE = 200      # Target words per chunk
MIN_SIZE = 50         # Minimum chunk size
MAX_SIZE = 350        # Maximum chunk size
OVERLAP = 2           # Sentence overlap between chunks
```

### Relevance Threshold

Edit in `fast_query_system.py`:

```python
if best_score < 0.35:  # Adjust threshold (0.0 - 1.0)
    return "❌ I don't know about this..."
```

**Threshold Guide:**
- `0.30` - More lenient (accepts more matches)
- `0.35` - Balanced (recommended)
- `0.40` - Strict (only very relevant answers)

### Pinecone Index Configuration

```python
INDEX_NAME = "medical-knowledge-v2"
EMBEDDING_MODEL = 'all-MiniLM-L6-v2'  # 384 dimensions
METRIC = "cosine"                      # Similarity metric
```

## ⚡ Performance

### Query Speed

| Query Type | First Query | Subsequent Queries |
|------------|-------------|-------------------|
| Simple `ask()` | 3-5 seconds | **< 0.5 seconds** ⚡ |
| Detailed `query()` | 3-5 seconds | **< 1 second** ⚡ |
| Batch (10 questions) | 5 seconds | **~3-4 seconds total** |

### Relevance Scores

| Question | Score | Status |
|----------|-------|--------|
| "What is anemia?" | 0.65 | ✅ Excellent |
| "What is plasma?" | 0.70 | ✅ Excellent |
| "Explain hemoglobin" | 0.58 | ✅ Good |
| "Random text" | 0.12 | ❌ Rejected |

### Resource Usage

- **Model size**: ~90 MB (all-MiniLM-L6-v2)
- **RAM usage**: ~500 MB during queries
- **Index size**: Depends on PDF size (~8,500 vectors for 1,200 pages)

## 📚 Examples

### Example 1: Basic Medical Query

```python
print(ask("What is plasma?"))
```

**Output:**
```
⚡ Answered in 0.456s (relevance: 0.702)

Plasma is the liquid component of blood that carries cells, nutrients, 
hormones, and waste products throughout the body. It makes up about 55% 
of total blood volume and consists of 90% water, along with proteins, 
electrolytes, and other dissolved substances...
```

### Example 2: Detailed Query with Sources

```python
print(query("What is anemia?", top_k=3))
```

**Output:**
```
Anemia is a blood disorder characterized by a deficiency of red blood 
cells and/or hemoglobin, leading to decreased oxygen delivery to body 
tissues and symptoms like fatigue, weakness, shortness of breath, and 
pale skin.

────────────────────────────────────────────────────────────
📚 Additional context:

[2] (relevance: 0.63)
Anemia is diagnosed when a blood test shows a hemoglobin value of 
less than 13.5 gm/dL in a man or less than 12.0 gm/dL in a woman...

[3] (relevance: 0.57)
Types of anemia include: Iron deficiency anemia (most common), 
Aplastic anemia (bone marrow disorder), Sickle cell anemia...

────────────────────────────────────────────────────────────
⚡ Query time: 0.856s | Best score: 0.650
```

### Example 3: Non-Medical Query (Filtered)

```python
print(ask("Who is the president?"))
```

**Output:**
```
⚡ Answered in 0.423s (relevance: 0.145)

❌ I don't know about this. Please ask medical questions related to 
the textbook content.
```

## 🛠️ Troubleshooting

### Issue: "ModuleNotFoundError: No module named 'fast_query_system'"

**Solution:** Run the code directly in Jupyter or save as `fast_query_system.py` in the same directory.

### Issue: "PINECONE_API_KEY not found"

**Solution:** Create a `.env` file with your API key:
```env
PINECONE_API_KEY=your_key_here
```

### Issue: Slow first query (> 5 seconds)

**Expected:** The first query loads the model (~3-5 seconds). All subsequent queries are < 1 second.

### Issue: Irrelevant answers

**Solutions:**
1. Lower the relevance threshold (e.g., 0.40 instead of 0.35)
2. Re-process PDF with smaller chunk sizes (150 words)
3. Use `query()` with higher `top_k` to see more context

### Issue: "Index not found"

**Solution:** Run `optimized_pipeline.py` first to create the Pinecone index.

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Development Guidelines

- Follow PEP 8 style guide
- Add docstrings to all functions
- Include unit tests for new features
- Update README.md with new features

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **OpenStax** - Medical terminology textbook source
- **Sentence Transformers** - Embedding model
- **Pinecone** - Vector database
- **PyMuPDF** - PDF text extraction

## 📧 Contact

**Your Name** - [@yourtwitter](https://twitter.com/yourtwitter)

Project Link: [https://github.com/yourusername/medical-chatbot](https://github.com/yourusername/medical-chatbot)

---

⭐ **If you find this project helpful, please give it a star!** ⭐

---

## 🔮 Future Enhancements

- [ ] Add GPT-based answer generation (LLM integration)
- [ ] Support multiple medical textbooks
- [ ] Add conversation history/memory
- [ ] Web UI with Streamlit/Gradio
- [ ] API endpoint with FastAPI
- [ ] Multi-language support
- [ ] Voice input/output
- [ ] Citation tracking and source references
- [ ] Fine-tuned medical domain embeddings
- [ ] Docker containerization

---

**Built with ❤️ for medical education and research**
