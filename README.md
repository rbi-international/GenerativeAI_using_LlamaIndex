# 📚 Generative AI with LlamaIndex, Chroma DB & PaLM

### How to run?
```bash
1. conda create -n llama python=3.13.2 -y

2. conda activate llama

3. pip install -r requirements.txt
```


This project is inspired by [Data Science with Bappy’s Udemy course](https://www.udemy.com/course/generative-ai-with-ai-agents-mcp-for-developers/learn/lecture/50262741#overview).
It shows how to build a local knowledge base using **LlamaIndex**, store embeddings efficiently with **ChromaDB**, and query them using an LLM like **OpenAI GPT** or **PaLM**.

## 🚀 Features

✅ Load documents from a local `data/` folder  
✅ Create a vector index (`VectorStoreIndex`)  
✅ Store embeddings in **ChromaDB**  
✅ Configure chunk size & LLM using **Settings**  
✅ Reuse saved indexes  
✅ Use OpenAI or Google PaLM as LLM  
✅ Query your knowledge base naturally  

## 🗂️ Project Structure

```
GenerativeAI_using_LlamaIndex/
├── data/                  # Your documents
├── chroma_db/             # ChromaDB storage
├── storage/               # LlamaIndex index storage
├── 01-llamaindex.ipynb    # Core notebook
├── 02-llamaindex-deep-dive.ipynb  # Advanced notebook
├── requirements.txt
├── README.md
```

## ⚙️ Quick Start

### 1️⃣ Install dependencies

```bash
pip install -r requirements.txt
pip install llama-index-vector-stores-chroma
pip install llama-index-llms-palm  # Optional, for PaLM
```

### 2️⃣ Prepare `.env`

```env
OPENAI_API_KEY="your_openai_api_key"
# For PaLM, set up Google Cloud credentials:
# export GOOGLE_APPLICATION_CREDENTIALS="path/to/your-service-account.json"
```

### 3️⃣ Run notebooks

Open:
- `01-llamaindex.ipynb`
- `02-llamaindex-deep-dive.ipynb`

## 🔑 Key Code

```python
from llama_index.core import SimpleDirectoryReader, VectorStoreIndex, StorageContext
from llama_index.vector_stores.chroma import ChromaVectorStore
import chromadb

# Load data
documents = SimpleDirectoryReader("data").load_data()

# Setup Chroma
chroma_client = chromadb.PersistentClient(path="./chroma_db")
chroma_collection = chroma_client.get_or_create_collection("quickstart")
vector_store = ChromaVectorStore(chroma_collection=chroma_collection)
storage_context = StorageContext.from_defaults(vector_store=vector_store)

# Create index
index = VectorStoreIndex.from_documents(documents, storage_context=storage_context)
index.storage_context.persist()

# Query
query_engine = index.as_query_engine(similarity_top_k=5)
response = query_engine.query("In less than 100 words, what is the meaning of good for the author?")
print(response)
```

## 📌 Reference

Based on the [Generative AI with AI Agents (MCP) for Developers](https://www.udemy.com/course/generative-ai-with-ai-agents-mcp-for-developers/learn/lecture/50262741#overview) course by **Data Science with Bappy**, extended with latest **LlamaIndex** best practices.

## ✅ Requirements

```txt
llama-index==0.12.39
llama-index-vector-stores-chroma
llama-index-llms-palm  # optional
chromadb==1.0.12
python-dotenv==1.1.0
```

## 🚀 Happy building!

