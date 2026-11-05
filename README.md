# Agent Development Kit (ADK) + Vertex AI RAG Engine

A production-ready Retrieval-Augmented Generation (RAG) engine built with Google's [Agent Development Kit (ADK)](https://google.github.io/adk-docs/) and [Vertex AI RAG Engine](https://cloud.google.com/vertex-ai/generative-ai/docs/rag-engine/rag-overview). This project provides a modular framework for managing Google Cloud Storage (GCS) buckets, RAG corpora, and document retrieval with a focus on best practices and user experience.

![RAG Query Interface](.Images/RAG-Single-Query-Search-Web.gif)

![GCS File Upload Interface](.Images/GCS-File-Upload-Web.gif)

## Vertex AI RAG Engine

Vertex AI RAG Engine is a component of the Vertex AI Platform that facilitates Retrieval-Augmented Generation (RAG) and serves as a data framework for developing context-augmented large language model (LLM) applications. It enables you to enrich LLM context with your organization's private knowledge, reducing hallucinations and improving answer accuracy.

### RAG Process Concepts

These concepts are listed in the order of the retrieval-augmented generation (RAG) process:

1. **Data ingestion**: Intake data from different data sources. For example, local files, Cloud Storage, and Google Drive.

2. **Data transformation**: Conversion of the data in preparation for indexing. For example, data is split into chunks.

3. **Embedding**: Numerical representations of words or pieces of text. These numbers capture the semantic meaning and context of the text. Similar or related words or text tend to have similar embeddings, which means they are closer together in the high-dimensional vector space.

4. **Data indexing**: Vertex AI RAG Engine creates an index called a corpus. The index structures the knowledge base so it's optimized for searching. For example, the index is like a detailed table of contents for a massive reference book.

5. **Retrieval**: When a user asks a question or provides a prompt, the retrieval component in Vertex AI RAG Engine searches through its knowledge base to find information that is relevant to the query.

6. **Generation**: The retrieved information becomes the context added to the original user query as a guide for the generative AI model to generate factually grounded and relevant responses.

## Agent Development Kit (ADK)

[Agent Development Kit (ADK)](https://google.github.io/adk-docs/) is a flexible and modular framework for developing and deploying AI agents. Key features include:

- **Model-Agnostic**: While optimized for Gemini and the Google ecosystem, ADK works with any model.
- **Flexible Orchestration**: Define workflows using workflow agents (`Sequential`, `Parallel`, `Loop`) or leverage LLM-driven dynamic routing for adaptive behavior.
- **Multi-Agent Architecture**: Build modular applications by composing multiple specialized agents in a hierarchy.
- **Rich Tool Ecosystem**: Equip agents with diverse capabilities through pre-built tools, custom functions, third-party integrations, or even other agents as tools.
- **Deployment Ready**: Deploy agents anywhere – locally, on Vertex AI Agent Engine, or using Cloud Run/Docker.
- **Built-in Evaluation**: Assess agent performance by evaluating both response quality and execution trajectory.

ADK makes agent development feel more like software development, making it easier to create, deploy, and orchestrate agents ranging from simple tasks to complex workflows.

## Table of Contents

- [Vertex AI RAG Engine](#vertex-ai-rag-engine)
  - [RAG Process Concepts](#rag-process-concepts)
- [Agent Development Kit (ADK)](#agent-development-kit-adk)
- [Features](#features)
- [Pre-created RAG Corpora](#pre-created-rag-corpora)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
  - [Running the Agent](#running-the-agent)
  - [Example Commands](#example-commands)
- [Configuration](#configuration)
- [Supported File Types](#supported-file-types)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)
- [References](#references)
- [Example Workflow](#example-workflow)
  - [Create GCS Buckets](#1-create-gcs-buckets)
  - [Upload PDF Files to GCS Buckets](#2-upload-pdf-files-to-gcs-buckets)
  - [Create RAG Corpora and Import Files](#3-create-rag-corpora-and-import-files) 
  - [Query Across All Corpora](#4-query-across-all-corpora)
- [Author](#author)

## Features

- 🗂️ **GCS Bucket Management**: Create, list, and manage GCS buckets for file storage.
- 📚 **RAG Corpus Management**: Create, update, list, and delete RAG corpora in Vertex AI.
- 📄 **Document Management**: Import documents from GCS into RAG corpora for vector search.
- 🔎 **Semantic Search**: Query RAG corpora for relevant information with citations.
- 🤖 **Agent-based Interface**: Interact with all functionalities through a natural language interface.
- ⚙️ **Configurable & Extensible**: Centralized configuration, emoji-enhanced responses, and schema-compliant tools.

## Pre-created RAG Corpora

The project includes several pre-created RAG corpora covering major AI topics:

- **Foundation Models & Prompt Engineering**: Resources on large language models and effective prompt design
- **Embeddings & Vector Stores**: Details on text embeddings and vector databases
- **Generative AI Agents**: Information on agent design, implementation, and usage
- **Domain-Specific LLMs**: Techniques for applying LLMs to solve domain-specific problems
- **MLOps for Generative AI**: Deployment and production considerations for GenAI systems

Each corpus contains relevant PDF documents imported from Google and Kaggle's Gen AI Intensive course:

- [Day 1: Foundational Models & Prompt Engineering](https://lnkd.in/d-_w3gXj)
- [Day 2: Embeddings & Vector Stores / Databases](https://lnkd.in/dkmfDUcp)
- [Day 3: Generative AI Agents](https://lnkd.in/dd3Zd2-F)
- [Day 4: Domain-Specific LLMs](https://lnkd.in/d6Z39yqt)
- [Day 5: MLOps for Generative AI](https://lnkd.in/dcXCTPVF)

These documents are from Google and Kaggle's Gen AI Intensive course, which broke the GUINNESS WORLD RECORDS™ title for the Largest Attendance at a Virtual AI Conference in One Week with more than 280,000 signups in just 20 days. The materials provide a comprehensive overview of Vertex AI capabilities and best practices for working with generative AI.

## Architecture

The project follows a modular architecture based on the ADK framework:

![ADK Vertex AI RAG Architecture](.Images/ADK-VertexAI-RAG-Architecture.png)

The architecture consists of several key components:

1. **User Interface**: Interact with the system through ADK Web or CLI
2. **Agent Development Kit (ADK)**: The core orchestration layer that manages tools and user interactions
3. **Function Tools**: Modular components divided into:
   - **Storage Tools**: For GCS bucket and file management
   - **RAG Corpus Tools**: For corpus management and semantic search
4. **Google Cloud Services**:
   - **Google Cloud Storage**: Stores document files
   - **Vertex AI RAG Engine**: Provides embedding, indexing and retrieval capabilities
   - **Gemini 2.0 LLM Model**: Generates responses grounded in retrieved contexts

File structure:
```
adk-vertex-ai-rag-engine/
├── rag/                          # Main project package
│   ├── __init__.py               # Package initialization
│   ├── agent.py                  # The main RAG corpus manager agent
│   ├── config/                   # Configuration directory
│   │   └── __init__.py           # Centralized configuration settings
│   └── tools/                    # ADK function tools
│       ├── __init__.py           # Tools package initialization
│       ├── corpus_tools.py       # RAG corpus management tools
│       └── storage_tools.py      # GCS bucket management tools
├── .Images/                      # Demo images and GIFs
└── README.md                     # Project documentation
```

## Prerequisites

- Python 3.11+
- Google Cloud project with Vertex AI API enabled
- [Google Cloud SDK](https://cloud.google.com/sdk/docs/install)
- Access to Vertex AI and Cloud Storage

## Installation

```bash
# Clone the repository
git clone https://github.com/arjunprabhulal/adk-vertex-ai-rag-engine.git
cd adk-vertex-ai-rag-engine

# (Optional) Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Configure your Google Cloud project
export GOOGLE_CLOUD_PROJECT="your-project-id"
export GOOGLE_CLOUD_LOCATION="us-central1"

# Enable required Google Cloud services
gcloud services enable aiplatform.googleapis.com --project=${GOOGLE_CLOUD_PROJECT}
gcloud services enable storage.googleapis.com --project=${GOOGLE_CLOUD_PROJECT}

# Set up IAM permissions
gcloud projects add-iam-policy-binding ${GOOGLE_CLOUD_PROJECT} \
    --member="user:YOUR_EMAIL@domain.com" \
    --role="roles/aiplatform.user"
gcloud projects add-iam-policy-binding ${GOOGLE_CLOUD_PROJECT} \
    --member="user:YOUR_EMAIL@domain.com" \
    --role="roles/storage.objectAdmin"

# Set up Gemini API key
# Get your API key from Google AI Studio: https://ai.google.dev/
export GOOGLE_API_KEY=your_gemini_api_key_here

# Set up authentication credentials
# Option 1: Use gcloud application-default credentials (recommended for development)
gcloud auth application-default login

# Option 2: Use a service account key (for production or CI/CD environments)
# Download your service account key from GCP Console and set the environment variable
export GOOGLE_APPLICATION_CREDENTIALS=/path/to/your/service-account-key.json
```

## Usage

### Running the Agent

There are two ways to run the agent:

```bash
# Option 1: Use ADK web interface (recommended for interactive usage)
adk web 

# Option 2: Run the agent directly in the terminal
adk run rag
```

The web interface provides a chat-like experience for interacting with the agent, while the direct run option is suitable for scripting and automated workflows.

### Example Commands

```
# List all GCS buckets
[user]: List all GCS buckets

# Create a bucket for Foundation LLMs
[user]: Create a GCS bucket named "adk-embedding-vector-stores"

# Upload a document
[user]: Upload this PDF file to GCS bucket gs://adk-embedding-vector-stores/ and keep the same destination blob name

# Create a RAG corpus
[user]: Create a RAG corpus named "adk-embedding-vector-stores" with description "adk-embedding-vector-stores"

# Import a document into RAG corpus
[user]: Import the file gs://adk-embedding-vector-stores/emebddings-vector-stores.pdf into the RAG corpus

# Query a specific RAG corpus about prompt engineering
[user]: What is Chain of Thought (CoT)?

# Query across all corpora about MLOps
[user]: How do multiple teams collaborate to operationalize GenAI models?
```

## Configuration

Edit `rag/config/__init__.py` to customize your settings:

- `PROJECT_ID`: Your Google Cloud project ID
- `LOCATION`: Default location for Vertex AI and GCS resources
- `GCS_DEFAULT_*`: Defaults for GCS operations
- `RAG_DEFAULT_*`: Defaults for RAG operations
- `AGENT_*`: Settings for the agent

## Supported File Types

The engine supports various document types, including:
- PDF
- TXT
- DOC/DOCX
- XLS/XLSX
- PPT/PPTX
- CSV
- JSON
- HTML
- Markdown

## Troubleshooting

### Common Issues

- **403 Errors**: Make sure you've authenticated with `gcloud auth application-default login`
- **Resource Exhausted**: Check your quota limits in the GCP Console
- **Upload Issues**: Ensure your file format is supported and file size is within limits

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

[MIT License](LICENSE)

## References

- [Google Agent Development Kit (ADK)](https://google.github.io/adk-docs/)
- [Vertex AI RAG Engine](https://cloud.google.com/vertex-ai/generative-ai/docs/rag-engine/rag-overview)
- [Google Cloud Storage](https://cloud.google.com/storage)
- [Building Agents with Google ADK 2.0](https://aiml.sh/courses/google-adk/): free hands-on course with runnable Python labs and tests (aiml.sh)
- [Building Agentic RAG Applications](https://aiml.sh/courses/agentic-rag/): free course on retrieval agents, reranking and RAG evaluation (aiml.sh)

## Example Workflow

Below is a complete example workflow showing how to set up the entire RAG environment with the Google Gen AI Intensive course materials:

### 1. Create GCS Buckets

![GCS Bucket Creation CLI](.Images/GCS-Bucket-creation-cli.gif)

```
Create the following 7 Google Cloud Storage buckets for my project, using the default settings (location: US, storage class: STANDARD) for all of them. Do not ask for confirmation for each bucket.

1. adk-foundation-llm
2. adk-prompt-engineering
3. adk-embedding-vector-stores
4. adk-agents-llm
5. adk-agents-companion
6. adk-solving-domain-problem-using-llms
7. adk-operationalizing-genai-vertex-ai
```

### 2. Upload PDF Files to GCS Buckets

![GCS File Upload Web](.Images/GCS-File-Upload-Web.gif)

![GCS Multiple Uploads](.Images/GCS-Multiple-Uploads.png)

```
Upload the file "promptengineering.pdf" to the GCS bucket gs://adk-prompt-engineering/ and use "promptengineering.pdf" as the destination blob name. Do not ask for confirmation.

Upload the file "foundational-large-language-models-text-generation.pdf" to the GCS bucket gs://adk-foundation-llm/ and use "foundational-large-language-models-text-generation.pdf" as the destination blob name. Do not ask for confirmation.

Upload the file "agents.pdf" to the GCS bucket gs://adk-agents-llm/ and use "agents.pdf" as the destination blob name. Do not ask for confirmation.

Upload the file "agents-companion.pdf" to the GCS bucket gs://adk-agents-companion/ and use "agents-companion.pdf" as the destination blob name. Do not ask for confirmation.

Upload the file "emebddings-vector-stores.pdf" to the GCS bucket gs://adk-embedding-vector-stores/ and use "emebddings-vector-stores.pdf" as the destination blob name. Do not ask for confirmation.

Upload the file "operationalizing-generative-ai-on-vertex-ai.pdf" to the GCS bucket gs://adk-operationalizing-genai-vertex-ai/ and use "operationalizing-generative-ai-on-vertex-ai.pdf" as the destination blob name. Do not ask for confirmation.

Upload the file "solving-domain-specific-problems-using-llms.pdf" to the GCS bucket gs://adk-solving-domain-problem-using-llms/ and use "solving-domain-specific-problems-using-llms.pdf" as the destination blob name. Do not ask for confirmation.
```

### 3. Create RAG Corpora and Import Files

![RAG Create Import Web](.Images/RAG-Create-Import-Web.gif)

![RAG Create Multiple Upload CLI](.Images/RAG-Create-Mutliple-Upload-CLI.gif)

```
Create a RAG corpus named "adk-agents-companion" with description of rag as "adk-agents-companion" and import the gs://adk-agents-companion/agents-companion.pdf into RAG

Create a RAG corpus named "adk-agents-llm" with description "adk-agents-llm" and import the file gs://adk-agents-llm/agents.pdf into the RAG corpus.

Create a RAG corpus named "adk-embedding-vector-stores" with description "adk-embedding-vector-stores" and import the file gs://adk-embedding-vector-stores/emebddings-vector-stores.pdf into the RAG corpus.

Create a RAG corpus named "adk-foundation-llm" with description "adk-foundation-llm" and import the file gs://adk-foundation-llm/foundational-large-language-models-text-generation.pdf into the RAG corpus.

Create a RAG corpus named "adk-operationalizing-genai-vertex-ai" with description "adk-operationalizing-genai-vertex-ai" and import the file gs://adk-operationalizing-genai-vertex-ai/operationalizing-generative-ai-on-vertex-ai.pdf into the RAG corpus.

Create a RAG corpus named "adk-solving-domain-problem-using-llms" with description "adk-solving-domain-problem-using-llms" and import the file gs://adk-solving-domain-problem-using-llms/solving-domain-specific-problems-using-llms.pdf into the RAG corpus.
```

### 4. Query Across All Corpora

![RAG Multiple Query Search CLI](.Images/RAG-Multiple-Query-Search-CLI.gif)

![RAG Multiple Search Corpus Web](.Images/RAG-Multiple-Search-Corpus-Web.gif)

![RAG Single Query Search Web](.Images/RAG-Single-Query-Search-Web.gif)

```
# Questions about Prompt Engineering
What is Chain of Thought (CoT)?
What is Tree of Thoughts (ToT)?
What is ReAct (reason & act)?

# Questions about Embeddings & Vector Stores
What are Types of embeddings?
What is Vector search?
What is Vector databases?

# Questions about Agents
What is Agent Lifecycle?

# Questions about MLOps & Operationalization
How do multiple teams collaborate to operationalize GenAI models?
How multiple teams collaborate to operationalize both models and GenAI applications?
```

## Author

For more articles on AI/ML and Generative AI, follow me on Medium: https://medium.com/@arjun-prabhulal