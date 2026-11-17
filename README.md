# Hybrid-RAG for Sparse Municipal Data Environments

**Master's Thesis Project** | *Friedrich-Alexander-Universität Erlangen-Nürnberg, Chair of Information Systems I*
**Author:** Berkant Cinar

---

## Overview

Municipal information about shared resources, such as workshops, devices and community spaces, is scattered across many websites with little structure, and citizens rarely use the same words as the official sources. This project addresses this problem with a **Hybrid Retrieval-Augmented Generation (RAG)** system that combines:

- a **domain ontology** built on CIDOC CRM (ISO 21127) and stored as a knowledge graph in **Neo4j**,
- **semantic vector search** over municipal web content and serialised graph facts in **ChromaDB**,
- **Meta Llama 3.1 8B Instruct** for intent extraction and answer generation.

The system was built and evaluated on data from 14 organisations of the Nürnberg innovation ecosystem. The full pipeline is implemented in `Hybrid_RAG_System_Notebook_Final.ipynb` and was tested on Google Colab with an A100 GPU.

## Related repository

The data sources, the domain ontology (OWL), the SPARQL competency question queries and their outputs are stored in a separate repository: [data repo linki]

## Architecture

- **Stages 1–2:** Data collection from municipal websites, cleaning and PDF generation
- **Stage 3:** Chunking and vector database creation with multilingual embeddings
- **Stage 4:** Knowledge graph creation in Neo4j from the ontology
- **Stage 5:** Knowledge graph serialisation and embedding in ChromaDB
- **Stage 6:** Multi-channel retrieval: document embeddings, Cypher queries on the knowledge graph and knowledge graph embeddings
- **Stage 7:** LLM-based response generation with source attribution

## Example

**Question:** "Which publicly accessible spaces are suitable for hosting small public meetings?"

**Result:** The system returned all three public spaces defined in the ontology, including the Event Hall of Heizhaus Nürnberg and KLARA Open, although none of them is labelled as a meeting room in the sources.

## Running on Google Colab

1. **Open the main notebook** `Hybrid_RAG_System_Notebook_Final.ipynb` in Google Colab.
2. **Configure credentials** in a `.env` file. Hugging Face access is needed to download the LLM, and Neo4j access to connect to the knowledge graph:
   ```
   HUGGINGFACEHUB_API_TOKEN=hf_your_token
   NEO4J_URI=neo4j+s://your-instance
   NEO4J_USERNAME=neo4j
   NEO4J_PASSWORD=your_password
   ```
3. **Run all cells in order.** The notebook automatically:
   - scrapes the municipal websites,
   - creates the vector embeddings,
   - imports and embeds the knowledge graph data,
   - runs multi-channel retrieval,
   - generates answers with source attribution.

## Requirements

```
langchain==1.2.0
langchain-community==0.4.1
langchain-text-splitters==1.1.0
langchain-core==1.2.6
sentence-transformers==5.2.0
chromadb==1.4.0
transformers==4.57.3
accelerate==1.12.0
bitsandbytes==0.49.0
torch==2.9.0+cu126
neo4j==6.0.3
pypdf==6.5.0
fpdf2==2.8.5
requests==2.32.5
beautifulsoup4==4.13.5
python-dotenv==1.2.1
ipykernel==6.17.1
```

## Citation

```bibtex
@mastersthesis{cinar2026hybrid,
  author = {Cinar, Berkant},
  title  = {Hybrid-RAG for Sparse Municipal Data Environments},
  school = {Friedrich-Alexander-Universität Erlangen-Nürnberg},
  year   = {2026}
}
```
