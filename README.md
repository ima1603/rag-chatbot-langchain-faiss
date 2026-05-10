# 🤖 CIVIC HELP BOT

An AI-powered conversational assistant designed specifically for democratizing access to Karnataka government schemes. This platform combines Retrieval-Augmented Generation (RAG) with semantic search to deliver accurate, document-backed responses while maintaining conversational context and intelligent fallback mechanisms for complex queries.

## 🚀 Core Philosophy

Government scheme information is fragmented across multiple sources, making it difficult for citizens to understand eligibility, benefits, and application procedures. CIVIC HELP BOT consolidates authoritative scheme data into a single conversational interface, powered by semantic search and large language models, ensuring responses are factually grounded in verified documents rather than generated guesses.

## 🛠️ Tech Stack

* **Frontend**: Streamlit (Python)
* **LLM Framework**: LangChain
* **Vector Database**: FAISS (CPU-based semantic search)
* **Text Embeddings**: Sentence Transformers
* **Language Model**: Google Gemini Flash (with Groq/OpenAI support)
* **Fallback Search**: Web search integration
* **Language**: Python

## ✨ Core Capabilities

* **Semantic Document Retrieval**: RAG pipeline retrieves relevant scheme chunks from indexed documents before LLM generation, ensuring answers are grounded in verified data.
* **Granular Query Classification**: Automatic routing of scheme-related vs. general queries using keyword-based detection and confidence scoring.
* **FAISS-Powered Vector Search**: Sub-second semantic similarity search across all indexed scheme documents using Sentence Transformer embeddings.
* **Intelligent Web Search Fallback**: Automatic fallback to web search when RAG confidence is low or no relevant documents are found, with formatted result assembly.
* **Conversational Memory**: Per-session chat history with quick-access buttons to previous queries, enabling multi-turn conversations with context preservation.
* **Response Mode Selection**: Users toggle between concise (bullet-point) and detailed (full-context) response modes for the same query.
* **Multi-LLM Abstraction**: Works seamlessly with Google Gemini Flash, Groq, and OpenAI via LangChain provider abstraction.

## 🗄️ Data Architecture

The system operates on a simple but effective document-indexing model:

1. **Scheme Documents** (`data/` folder): Raw text/PDF files containing scheme details, eligibility criteria, benefits, and application procedures. Manually curated for accuracy.
2. **Embedding Pipeline**: Documents are split into chunks, converted to 384-dimensional vectors using Sentence Transformers, and indexed in FAISS.
3. **Query Matching**: User queries are embedded using the same model and matched against the FAISS index for top-k similarity results.
4. **Context Assembly**: Retrieved chunks are concatenated with metadata and passed to the LLM along with the original query for synthesis.
5. **Confidence Scoring**: Similarity scores from FAISS are used to determine whether to return RAG results or trigger web search fallback.


## 🗺️ Implementation Status

* **Phase 1 (Current MVP)**: Manual document ingestion, keyword-based query routing, FAISS semantic search, LLM response synthesis with web search fallback, Streamlit UI with response mode toggle.
* **Phase 2 (Planned)**: Vision LLM pipeline for automated PDF parsing, extraction of complex tables and mathematical content, structured data ingestion.
* **Phase 3 (Planned)**: Confidence scoring mechanism, query complexity stratification, session-based performance analytics.

## 📊 Supported Schemes (Current)

* **Sanjeevini Scheme** - Government health insurance and medical benefits
* **NRLM** - National Rural Livelihood Mission for livelihood enhancement
* Additional schemes can be added by placing documents in the `data/` folder

## 🔄 How It Works

**User Query Flow:**
```
User enters query in Streamlit chat
        ↓
Query classified as scheme-related or general
        ↓
   [IF SCHEME-RELATED]              [IF GENERAL]
   FAISS retrieves top-k documents  → Web search
   LLM synthesizes with context
        ↓
   Is confidence high? 
        ↓ No (confidence < threshold)
        → Web search for supplementary results
        ↓
   Formatted response displayed with source attribution
```

**Internal Processing:**
- Document embedding: Sentence Transformers (384 dims)
- Similarity search: FAISS cosine similarity
- Response synthesis: LLM prompt = [system_prompt + retrieved_context + user_query]
- Latency: ~0.3s search + ~1.0s LLM + ~0.2s overhead = ~1.5s average

## 🎯 Key Files

* `app.py` - Main Streamlit application, chat interface, session management
* `models/llm.py` - LLM initialization and invocation (Google Gemini Flash)
* `utils/rag_utils.py` - Document loading, embedding, FAISS index creation, retrieval logic
* `utils/websearch.py` - Web search integration and result formatting
* `config/config.py` - API key management and configuration
