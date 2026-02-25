# RAG Without Vectors

# The First Reasoning-Native Architecture for Long-Form Document Intelligence

Traditional Retrieval-Augmented Generation (RAG) systems struggle with complex, long-form documents such as SEC filings, annual reports, and technical manuals. Semantic similarity does **not** equal relevance.

Queries like:

> *“What are the financial vulnerabilities?”*

often retrieve sections about *market trends* due to embedding overlap — resulting in:

* ❌ 30%+ irrelevant context
* ❌ No multi-hop reasoning (e.g., cross-referencing tables/appendices)
* ❌ Poor explainability
* ❌ 75–85% benchmark accuracy (e.g., FinanceBench)

---

# Solution :

**PageIndex technique eliminates vectors, embeddings, and chunking entirely.**

Instead of breaking documents into fragments, it builds a **hierarchical reasoning tree** from PDFs or Markdown files. Each node captures:

* Title
* Page range
* Summary
* Child nodes

Retrieval becomes **agentic tree search**, not similarity search.

---

## 🏗 Architecture Overview

```
PDF / Markdown
        ↓
Tree Index Generation (LLM-driven hierarchical reasoning)
        ↓
User Query
        ↓
Agentic Tree Search
  → Evaluate node relevance
  → Expand children
  → Rank paths
  → Follow references
        ↓
Retrieved Context
        ↓
RAG Generation
```

---

## 📊 Benchmark Results

On FinanceBench:

| System                 | Accuracy  |
| ---------------------- | --------- |
| Traditional Vector RAG | ~85%      |
| **PageIndex.ai**       | **98.7%** |

With full traceability:

```
Answer sourced from pages 22–28  
Path: Financial Stability → Monitoring Vulnerabilities
```

---

# 🔍 Why PageIndex Works

| Aspect           | Traditional Vector RAG       | PageIndex.ai                 |
| ---------------- | ---------------------------- | ---------------------------- |
| Storage          | Vector DB (FAISS / Pinecone) | Tree JSON (~10× smaller)     |
| Indexing         | Chunk + Embed (5–10s/doc)    | Tree Build (1–15 min/doc)    |
| Retrieval        | kNN similarity               | Reasoning-based tree search  |
| Explainability   | Similarity scores            | Page paths + reasoning trace |
| Multi-turn QA    | Stateless                    | Chat-history aware           |
| Cross-references | Weak                         | Native support               |
| FinanceBench     | ~85%                         | **98.7%**                    |

---

# 🌳 Example Tree Node

```json
{
  "title": "Financial Stability",
  "node_id": "0006",
  "start_index": 21,
  "end_index": 22,
  "summary": "Federal Reserve monitoring framework...",
  "nodes": [
    {
      "title": "Monitoring Vulnerabilities",
      "node_id": "0007"
    }
  ]
}
```

---

# ⚙️ Core Advantages

### ✅ No Vector Database

No embeddings. No Pinecone. No FAISS. Zero vector storage cost.

### ✅ No Chunking

Preserves natural structure: sections, tables, appendices, and references like *“See Section 3.2”*.

### ✅ Human-like Navigation

Simulates expert reading behavior:

> Start at TOC → Follow relevant path → Drill into leaf nodes → Extract evidence

### ✅ Vision Support

OCR-free RAG directly from PDF page images.

### ✅ Production Ready

* Managed Chat
* API Access
* MCP Integrations
* Self-hosted deployment

---

# 💰 Cost & Performance Optimization

### Primary Bottleneck

LLM calls for:

* Tree generation (50k–200k tokens per 100-page doc)
* Iterative retrieval reasoning

Using Groq-hosted OSS models dramatically reduces costs.

## Optimized Pricing (Feb 2026)

| Model             | Provider | Cost (₹ / 1M tokens) | 100pg Indexing    | Latency  |
| ----------------- | -------- | -------------------- | ----------------- | -------- |
| GPT-4o            | OpenAI   | 210                  | ₹85–420 (10 min)  | High     |
| **Llama 3.1 70B** | Groq     | **8.5**              | **₹4–17 (2 min)** | Low      |
| Mixtral 8×22B     | Groq     | 8.5                  | ₹4–10 (1 min)     | Very Low |
| Gemma 2 27B       | Groq     | 4.2                  | ₹2–8 (<1 min)     | Fastest  |

Example configuration:

```bash
--model llama-3.1-70b-versatile
GROQ_API_KEY=your_key
```

Achieves **90%+ of proprietary model accuracy** on structured reasoning tasks at a fraction of the cost.

---

# 📦 Quickstart

```bash
git clone https://github.com/VectifyAI/PageIndex
cd PageIndex
pip install -r requirements.txt

# Configure Groq
echo "GROQ_API_KEY=your_key" > .env

# Index PDF
python run_pageindex.py \
  --pdf_path report.pdf \
  --model llama-3.1-70b-versatile
```

Output:

```
tree.json
```

Ready for integration into your RAG pipeline.

---

# 🧠 When to Use PageIndex

### Ideal For:

* Annual reports
* SEC filings
* Regulatory documents
* Technical manuals
* Research papers
* Single or few large documents (<600 pages)

### Consider Hybrid (Vector + Tree) If:

* Massive corpora (>10,000 documents)
* Cross-document retrieval at web scale

---

# 🏭 Production Deployment

* 💬 Managed Chat
* 🔌 API Access
* 🐳 Docker Self-host
* ☁ Groq / Cloudflare Workers
* 🏢 Enterprise Private Deployments

---

Index retrieves understanding.**
