# 🔍 Agent RAG Pipeline 

This tutorial demonstrates how to build a Retrieval-Augmented Generation (RAG) pipeline using LangChain and OpenAI models, enhanced with LangGraph for structured agent workflows.

---

## 🧱 Key Components

- **Environment Setup**
  - Loads API keys securely using `dotenv`
  - Initializes OpenAI API for chat and embedding models

- **Chat Model**
  - Uses `ChatOpenAI` to handle simple user queries

- **Web Document Loading**
  - Fetches content from web sources via `WebBaseLoader`
  - Allows inspection of metadata and previewing document content

- **Text Splitting**
  - Breaks long documents into smaller, overlapping chunks using `RecursiveCharacterTextSplitter`

- **Embedding & Vector Store**
  - Embeds chunks using `OpenAIEmbeddings`
  - Stores vectors in a `Chroma` database for similarity search

- **Retriever**
  - Converts the vector store into a retriever interface
  - Enables semantic similarity-based document retrieval

- **Retriever Tool for LangGraph**
  - Wraps the retriever as a `retriever_tool` with a descriptive interface
  - Registers it as a `ToolNode` for use within LangGraph workflows

---

## 🔧 LangGraph Agent Workflow

The intelligent agent is built using LangGraph, consisting of modular nodes and conditional transitions to support decision-making and dynamic query handling.

### 🧩 Core Nodes

- **LLM Decision Maker**
  - Evaluates initial user input
  - Decides whether to respond directly or use a tool (retriever)

- **Vector Retriever**
  - Retrieves relevant documents from the vector store

- **Grading Node**
  - Evaluates whether retrieved content is relevant to the user’s query
  - Routes flow to either the output generator or query rewriter

- **Output Generator**
  - Uses RAG-style prompts to generate a detailed final answer

- **Query Rewriter**
  - Reformulates vague or off-target questions and sends them back to the decision node

### 🔄 Flow Logic

1. The process begins at the `LLM Decision Maker`
2. If a tool is needed, it transitions to `Vector Retriever`
3. The `grade_documents` function determines:
   - ✅ If relevant → move to `Output Generator` → then END
   - ❌ If not relevant → move to `Query Rewriter` → loops back to `LLM Decision Maker`

---

This architecture provides a flexible and intelligent QA pipeline, leveraging both LLM capabilities and retriever-based context augmentation with built-in reasoning and recovery loops.
