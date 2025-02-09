# Setting Up LlamaIndex with a Local Hugging Face Model (No OpenAI API)

This guide will walk you through setting up **LlamaIndex** on an Ubuntu system with a **local Hugging Face embedding model** instead of OpenAI API. By the end, you will have a working system that can index and query documents efficiently.

---

## **1. Prerequisites**
Before getting started, make sure you have:
- **Ubuntu (or any Linux system)**
- **Python 3.12+ installed** (Check with `python3 --version`)
- **A virtual environment to manage dependencies**

If you don’t have Python installed, install it with:
```bash
sudo apt update && sudo apt install python3 python3-venv python3-pip -y
```

---

## **2. Create and Activate a Virtual Environment**
LlamaIndex and its dependencies should be installed in a virtual environment to avoid conflicts.

```bash
python3 -m venv llama_env
source llama_env/bin/activate  # Activate environment
```

---

## **3. Install Required Packages**
Inside the activated virtual environment, install the necessary Python packages:

```bash
pip install llama-index llama-index-embeddings-huggingface transformers torch
```

This installs:
- **LlamaIndex** (for indexing and querying documents)
- **Hugging Face embedding model support**
- **Transformers & Torch** (required for embedding models)

---

## **4. Prepare Data for Indexing**
LlamaIndex needs a directory with documents to process. Create a `data/` directory and add sample files.

```bash
mkdir data
```

Add sample text files:
```bash
echo "LlamaIndex is a tool for indexing and retrieving documents." > data/info.txt
echo "This is a test document for embedding models." > data/test.txt
```

You can also manually add PDFs, CSVs, or other text-based documents to this folder.

---

## **5. Create a Python Script for Indexing and Querying**
Create a Python file called `query_llama.py`:

```bash
nano query_llama.py
```

Paste the following Python script:

```python
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader, Settings
from llama_index.embeddings.huggingface import HuggingFaceEmbedding

# Disable OpenAI LLM to avoid API issues
Settings.llm = None

# Load local Hugging Face embedding model
embed_model = HuggingFaceEmbedding(model_name="sentence-transformers/all-MiniLM-L6-v2")

# Load documents from the 'data' folder
documents = SimpleDirectoryReader("data").load_data()

# Create an index using the local embedding model
index = VectorStoreIndex.from_documents(documents, embed_model=embed_model)

# Create a query engine (without OpenAI LLM)
query_engine = index.as_query_engine()

# Perform a query
response = query_engine.query("What information is in these files?")
print(response)
```

Save and exit the file (**CTRL+X**, then **Y**, then **Enter**).

---

## **6. Run the Script**
Execute the script to index the documents and perform a query:
```bash
python query_llama.py
```

Expected Output (May Vary):
```bash
LLM is explicitly disabled. Using MockLLM.
Context information is below.
---------------------
file_path: /home/user/data/info.txt
file_path: /home/user/data/test.txt

LlamaIndex is a tool for indexing and retrieving documents.
This is a test document for embedding models.
---------------------
Given the context information and not prior knowledge, answer the query.
Query: What information is in these files?
Answer: LlamaIndex is a tool for indexing and retrieving documents. This is a test document for embedding models.
```

---

## **7. Troubleshooting**
If you run into issues:
- **Check if data directory exists** (`ls data/`)
- **Ensure text files contain enough information** (Avoid empty responses)
- **Reset LlamaIndex settings if needed**:
```bash
rm -rf ~/.llama_index
```

---

## **8. Conclusion**
You have successfully:
✅ Installed LlamaIndex in a virtual environment  
✅ Used a **local Hugging Face embedding model**  
✅ Indexed and queried documents without using OpenAI API  

Now you can integrate this into larger AI/ML projects. 🚀

