# Documenation for PetroDoc AI
# PetroDoc AI: Intelligent Oil & Gas Document Pipeline

<p align="center">
  <strong>Automated Processing of Oil and Gas Files using AI.</strong>
</p>

<p align="center">
  <a href="https://www.python.org/downloads/"><img src="https://img.shields.io/badge/Python-3.9+-blue.svg" alt="Python 3.9+"></a>
  <a href="https://deepmind.google/technologies/gemini/"><img src="https://img.shields.io/badge/Google-Gemini-blue.svg" alt="Google Gemini"></a>
  <a href="https://python.langchain.com/"><img src="https://img.shields.io/badge/Langchain-framework-lightgrey.svg" alt="Langchain"></a>
  <a href="https://www.trychroma.com/"><img src="https://img.shields.io/badge/ChromaDB-vector--database-brightgreen.svg" alt="ChromaDB"></a>
  <a href="https://cloud.google.com/vision"><img src="https://img.shields.io/badge/Google%20Cloud-Vision%20AI-blue.svg" alt="Google Cloud Vision"></a>
  <a href="https://developers.google.com/drive/api"><img src="https://img.shields.io/badge/Google-Drive%20API-green.svg" alt="Google Drive API"></a>
  <a href="https://developers.google.com/docs/api"><img src="https://img.shields.io/badge/Google-Docs%20API-blue.svg" alt="Google Docs API"></a>
  <a href="https://cloud.google.com/storage"><img src="https://img.shields.io/badge/Google%20Cloud-Storage-blue.svg" alt="Google Cloud Storage"></a>
  <a href="https://pandas.pydata.org/"><img src="https://img.shields.io/badge/Pandas-data%20handling-lightgrey.svg" alt="Pandas"></a>
  <a href="https://lasio.readthedocs.io/"><img src="https://img.shields.io/badge/lasio-LAS%20parsing-lightgrey.svg" alt="lasio"></a>
  <a href="https://dlisio.readthedocs.io/"><img src="https://img.shields.io/badge/dlisio-DLIS%20parsing-lightgrey.svg" alt="dlisio"></a>
  <a href="https://segyio.readthedocs.io/"><img src="https://img.shields.io/badge/segyio-SEGY%20parsing-lightgrey.svg" alt="segyio"></a>
  <a href="https://python-pptx.readthedocs.io/"><img src="https://img.shields.io/badge/python--pptx-PPTX%20parsing-orange.svg" alt="python-pptx"></a>
  <a href="https://textract.readthedocs.io/"><img src="https://img.shields.io/badge/textract-fallback%20parsing-lightgrey.svg" alt="textract"></a>
  <a href="https://python-pillow.org/"><img src="https://img.shields.io/badge/Pillow-image%20proc-yellow.svg" alt="Pillow"></a>
  <a href="https://github.com/Belval/pdf2image"><img src="https://img.shields.io/badge/pdf2image-PDF%20conv-yellow.svg" alt="pdf2image"></a>
  <a href="https://www.djangoproject.com/"><img src="https://img.shields.io/badge/Django-backend-darkgreen.svg" alt="Django"></a>
  <a href="https://react.dev/"><img src="https://img.shields.io/badge/React-frontend-blue.svg" alt="React"></a>
  <a href="https://www.heroku.com/"><img src="https://img.shields.io/badge/Heroku-deployment-purple.svg" alt="Heroku"></a>
  <a href="https://redis.io/"><img src="https://img.shields.io/badge/Redis-cache%20%26%20broker-red.svg" alt="Redis"></a>
  <a href="https://docs.celeryq.dev/"><img src="https://img.shields.io/badge/Celery-task%20queue-green.svg" alt="Celery"></a>
  <a href="https://aws.amazon.com/s3/"><img src="https://img.shields.io/badge/AWS-S3%20Storage-orange.svg" alt="AWS S3"></a>
</p>

---

PetroDoc AI is a sophisticated Python application designed to parse a wide array of document formats prevalent in the Oil and Gas sector and analyze their content using cutting-edge AI. It extracts critical information, categorizes documents, and provides concise summaries, streamlining workflows and enhancing data discovery.

This system comprises two core Python modules:

1. **`FileParser` (`file_parser.py`):** A robust, multi-strategy engine for text and metadata extraction from complex file types including PDFs, Office documents, images (TIFF), and specialized geological/geophysical formats (LAS, DLIS, SEGY, PDS).

2. **`OilGasDocumentAnalyzer` (`analyze.py`):** An intelligent analysis layer leveraging Google's Gemini LLM and vector databases (ChromaDB) via Langchain for document categorization, entity extraction (wells, fields, concepts), database-backed verification, and summarization.

---

## Table of Contents

* [Key Features](#key-features)
* [Technology Stack & Rationale](#technology-stack--rationale)
  * [Core Technologies](#core-technologies)
  * [Architectural Decisions](#architectural-decisions)
  * [API Selection Rationale](#api-selection-rationale)
* [Installation & Setup](#installation--setup)
  * [Prerequisites](#prerequisites)
  * [Google Cloud Setup](#google-cloud-setup)
  * [Installation Steps](#installation-steps)
  * [Dependency Management](#dependency-management)
* [Configuration Deep Dive](#configuration-deep-dive)
* [Usage Guide](#usage-guide)
* [Processing Pipeline](#processing-pipeline)
  * [Logical Flow for `file_parser.py`](#logical-flow-file-parser)
  * [Logical Flow for `analyze.py`](#logical-flow-analyze)
  * [Data Flow](#data-flow)
* [Deployment Guide (Heroku)](#deployment-guide-heroku)
  * [Procfile Configuration](#procfile-configuration)
  * [Dependencies (System & Python)](#dependencies-system--python)
  * [Configuration Variables](#configuration-variables)
  * [Buildpacks & Aptfile](#buildpacks--aptfile)
  * [Limitations & Considerations](#limitations--considerations)

* [Model Cost Analysis](#model-cost-analysis)
  * [Note](#notes)
  * [1. Approach: File Type Categorization](#approach)
  * [2. Scenarios Assumptions ](#scenarios-assumptions)
  * [3. Cost Calculation Breakdown](#cost-calculation-breakdown)
  * [4. Summary Table](#summary-table)


* [Critical Recommendations & Best Practices](#critical-recommendations--best-practices)
  * [Asynchronous Processing](#asynchronous-processing)
  * [Scalable File Storage](#scalable-file-storage)
  * [Configuration Management](#configuration-management)
  * [Security Considerations](#security-considerations)
  * [Database Optimization](#database-optimization)
  * [Resource Management](#resource-management)
* [Troubleshooting](#troubleshooting)
* [Contributing](#contributing)
* [License](#license)

---

## Key Features

### File Parsing (`FileParser`)

* **Extensive Format Support:** `.pdf`, `.doc`, `.docx`, `.ppt`, `.pptx`, `.xls`, `.xlsx`, `.tif`, `.tiff`, `.las`, `.dlis`, `.segy`, `.sgy`, `.pds`, `.csv`, `.txt`. Handles both text-based and scanned/image-based documents.
* **High-Accuracy OCR:** Leverages Google Cloud Vision API for superior text extraction from scanned PDFs and TIFF images, critical for legacy documents.
* **Robust Conversion:** Utilizes Google Drive/Docs APIs as a reliable fallback for accurately converting proprietary legacy Microsoft Office formats (`.doc`, `.ppt`, `.xls`) into modern, processable formats.
* **Specialized Parsers:** Integrates dedicated libraries (`lasio`, `dlisio`, `segyio`) for extracting structured headers, metadata, and curve information from industry-specific formats.
* **Intelligent Tabular Data Handling:** Employs `pandas` for CSV/Excel with sophisticated auto-detection for delimiters and character encodings, plus intelligent sampling strategies for very large files to manage memory and processing time.
* **Resilient Fallbacks:** Incorporates local command-line tools (`textract`, `antiword`, `catppt` - requires installation) for DOC/PPT processing as a fallback, ensuring functionality even without cloud connectivity for certain types.
* **Performant Caching:** Implements configurable file-hash-based caching (`pickle` by default) to dramatically speed up reprocessing of identical files.

### Document Analysis (`OilGasDocumentAnalyzer`)

* **Hybrid Document Categorization:** Fuses LLM zero-shot capabilities (Gemini) with Retrieval-Augmented Generation (RAG) against user-defined categories and examples (vectorized via ChromaDB and Langchain). This provides both flexibility and grounding in domain-specific knowledge for accurate classification.
* **Contextual Entity Extraction:** Leverages Google Gemini's advanced language understanding to identify potential well names, primary wells, field names, and key technical concepts, considering the surrounding text for better disambiguation.
* **Database-Grounded Verification:** Implements a crucial verification loop. Extracted well and field entities are cross-referenced against a user-provided database (vectorized in ChromaDB). The system prioritizes database information linked to high-confidence well prefix matches, mitigating LLM hallucinations and standardizing entity representation.
* **LLM-Powered Summarization:** Generates concise (5-30 words), abstractive summaries capturing the document's core technical essence using Gemini, going beyond simple keyword extraction.
* **Configurable & Tunable:** Allows specification of different Gemini models, API keys, cache locations, and similarity thresholds for well verification, enabling fine-tuning for specific datasets and accuracy requirements.
* **Analysis Caching:** Caches final analysis results based on the hash of the input text, preventing redundant API calls and computation for previously analyzed content.

---

## Technology Stack & Rationale

This project leverages a carefully selected stack to balance performance, accuracy, and development efficiency for complex document processing in the O&G domain.

### Core Technologies

* **Python 3.9+:** Primary language; excellent ecosystem for AI/ML, data science, web backends.
* **Google Gemini (via `google-generativeai`, `langchain-google-genai`):** SOTA LLMs for complex NLU tasks (categorization, extraction, summarization). Chosen for instruction following and structured output proficiency.
* **Langchain Framework:** Orchestration layer simplifying LLM interactions, prompt management, RAG implementation, and component chaining.
* **Google Cloud Vision API:** Best-in-class OCR for scanned/complex PDFs and images, critical for legacy document digitization. Scalable and accurate.
* **Google Drive & Docs APIs:** Reliable server-side conversion engine for legacy MS Office formats, often superior to local libraries for complex layouts.
* **Google Cloud Storage (GCS):** Essential temporary storage for Vision API's asynchronous processing. Recommended for primary, scalable user file storage.
* **ChromaDB:** Open-source vector database. Selected for ease of use, embeddability (suitable for development/prototyping), Langchain integration, enabling RAG and entity verification.
* **Pandas:** De-facto standard for tabular data manipulation (CSV/Excel). Robust I/O and data cleaning capabilities.
* **Specialized O&G Libraries:**
  * `lasio`: Standard for `.las` (Log ASCII Standard) file parsing.
  * `dlisio`: Standard for `.dlis` (Digital Log Interchange Standard) file parsing.
  * `segyio`: Standard for `.segy`/`.sgy` (SEG-Y seismic data) header parsing.
* **File Processing Libraries:**
  * `textract`: Versatile library aggregating multiple text extraction methods (requires system tools).
  * `python-pptx`: Direct `.pptx` parsing fallback.
  * `pdf2image`: PDF page-to-image conversion (requires `poppler`).
  * `Pillow (PIL)`: Foundational image processing.
  * `xlrd` / `openpyxl`: Pandas engines for `.xls` / `.xlsx`.
* **Web Stack (To be Integrated):**
  * **Django:** Mature, full-featured Python framework for building the backend API.
  * **React:** Leading JavaScript library for building dynamic, interactive user interfaces.
* **Deployment Target:**
  * **Heroku:** Popular PaaS providing managed infrastructure for web apps and workers, simplifying deployment.
* **Recommended Enhancement Stack:**
  * **Redis:** High-performance in-memory cache and message broker.
  * **Celery:** Distributed task queue for asynchronous background processing.
  * **AWS S3:** Cloud object storage service.

### Architectural Decisions

* **Why Google Gemini?** Chosen for its advanced reasoning, instruction following, and ability to generate structured (JSON) output reliably, which is crucial for extracting specific entities and categorizations. Its performance on summarization tasks is also a key benefit. And finally its low cost and speed, as compared to alternatives (like GPT-4).
* **Why Google Cloud Vision API?** Selected for its unmatched OCR accuracy on diverse and often low-quality scanned documents common in the industry, significantly reducing the need for manual pre-processing compared to alternatives like Tesseract. The asynchronous API scales well.
* **Why Google Drive/Docs API for Conversion?** Provides the most reliable method for handling complex layouts and features within legacy Microsoft Office binary formats (`.doc`, `.ppt`, `.xls`), ensuring maximum content fidelity during conversion for subsequent analysis.
* **Why ChromaDB?** Offers a developer-friendly entry point into vector databases, essential for grounding LLM results (RAG) and implementing the entity verification logic against a domain-specific knowledge base (well/field database). Its seamless Langchain integration simplifies development.
* **Why Fallback Mechanisms (`FileParser`)?** Real-world documents are inconsistent. Combining cloud APIs with local libraries and command-line tools creates a more resilient parsing pipeline that maximizes the chance of successfully extracting text even if one method fails or is unavailable.
* **Why Separate Parser & Analyzer?** This modular design promotes separation of concerns. The `FileParser` can be independently tested, updated, or even reused, while the `OilGasDocumentAnalyzer` focuses purely on interpreting the extracted text.

### API Selection Rationale

API choices were deliberate to maximize accuracy, reliability, and development velocity:

#### Google Gemini

* **Reasoning Capabilities:** Demonstrates exceptional contextual understanding of technical oil & gas terminology and relationships between entities.
* **Structured Output:** Reliably produces consistent JSON structures, crucial for programmatic consumption of results.
* **Instruction Following:** Exhibits superior ability to follow complex multi-part instructions, essential for our nuanced entity extraction and verification pipelines.
* **Performance:** Provides faster response times than comparable alternatives at similar price points.
* **Model Variety:** Offers different model sizes (Flash, Pro), enabling cost/accuracy optimization based on the specific task complexity.

#### Google Cloud Vision

* **OCR Quality:** Leads the industry in extracting text from degraded, low-quality scans common in legacy technical documents.
* **Layout Understanding:** Maintains text flow and paragraph structures significantly better than open-source alternatives.
* **Table Recognition:** Correctly identifies and preserves tabular data structures crucial for technical documents.
* **Scale:** Handles large batches of documents with its asynchronous API, vital for processing document backlogs.
* **Language Support:** Provides excellent recognition across multiple languages found in international oil & gas operations.

#### Google Drive/Docs APIs

* **Format Fidelity:** Consistently converts complex Microsoft Office binary formats with superior layout preservation.
* **Server-side Processing:** Offloads heavy conversion tasks from application servers to Google's infrastructure.
* **Stability:** Maintains compatibility with even the most complex documents containing macros, embedded objects, and non-standard formatting.
* **Security:** Integrates with Google's authentication infrastructure, providing enterprise-grade document handling security.

#### Google Cloud Storage

* **Integration:** Seamlessly works with other Google Cloud services, particularly Vision API's batch processing.
* **Performance:** Offers high throughput and low latency, essential for document processing pipelines.
* **Lifecycle Management:** Provides automatic file expiration policies, useful for managing temporary processing artifacts.
* **Global Reach:** Enables multi-region deployment with consistent performance.

---

## Installation & Setup

Follow these steps to set up the PetroDoc AI environment.

### Prerequisites

1. **Python:** Version 3.9 or later is recommended. Check with `python --version`.
2. **Git:** Required for cloning the repository.
3. **System Packages:** Install necessary libraries for file processing and fallbacks using your system's package manager:
   * **Poppler:** Provides PDF rendering utilities needed by `pdf2image`.
     * *Debian/Ubuntu:* `sudo apt-get update && sudo apt-get install poppler-utils`
     * *MacOS:* `brew install poppler`
     * *Windows:* Download binaries, install, and ensure the `bin\` directory is in the system `PATH`.
   * **Antiword (Optional, for `.doc` fallback):**
     * *Debian/Ubuntu:* `sudo apt-get install antiword`
     * *MacOS:* `brew install antiword`
   * **Catdoc (Optional, provides `catppt` for `.ppt` fallback):**
     * *Debian/Ubuntu:* `sudo apt-get install catdoc`
     * *MacOS:* `brew install catdoc`

### Google Cloud Setup

1. **GCP Project:** Create or use an existing Google Cloud Project ([console.cloud.google.com](https://console.cloud.google.com/)).
2. **Enable APIs:** Navigate to "APIs & Services" -> "Library" and enable:
   * Google Drive API
   * Google Docs API
   * Cloud Vision API
   * Cloud Storage API (usually enabled by default)
3. **Service Account:**
   * Navigate to "IAM & Admin" -> "Service Accounts".
   * Click "Create Service Account", provide a name (e.g., petrodoc-ai-sa), and grant necessary roles:
     * Storage Object Admin (for reading/writing to the GCS bucket)
     * Cloud Vision AI User (to use the Vision API)
     * *(Optional)* Consider more granular roles or Drive/Docs specific roles if security requires.
   * Create a key for the service account (JSON type) and download the key file. **Secure this file!**
4. **GCS Bucket:**
   * Navigate to "Cloud Storage" -> "Buckets".
   * Click "Create Bucket", choose a **globally unique name**, select a region, and configure other settings as needed (Standard storage class is usually fine).
   * Ensure the Service Account created above has permissions (e.g., via the Storage Object Admin role) to access this bucket.

### Installation Steps

1. **Clone Repository:**
   ```bash
   gh repo clone RightClickITSolutions/petrodoc
   cd petrodoc
   ```

2. **Create & Activate Virtual Environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # Linux/macOS
   # venv\Scripts\activate  # Windows CMD
   # venv\Scripts\Activate.ps1 # Windows PowerShell
   ```

3. **Configure Environment Variables:** Create a `.env` file in the project root (add `.env` to `.gitignore`) or set system environment variables:
   ```
   # .env file example
   GEMINI_API_KEY="YOUR_GOOGLE_GEMINI_API_KEY"

   # --- Choose ONE way to provide service account credentials ---
   # Option 1: Path (Recommended for local development)
   GOOGLE_APPLICATION_CREDENTIALS="/full/path/to/your/downloaded-service-account.json"
   # Option 2: Base64 encoded content (Recommended for deployment like Heroku)
   # GOOGLE_APPLICATION_CREDENTIALS_CONTENT="ewoJInR5cGUiOiAic2Vydml...<rest of base64 string>...ifQo="

   GCS_BUCKET_NAME="your-globally-unique-gcs-bucket-name"

   ```
   *(Remember to install python-dotenv: `pip install python-dotenv` and use `load_dotenv()` in your main script)*

### Setting up the Gemini API Key (via Google AI Studio) & Heroku Configuration

The `OilGasDocumentAnalyzer` uses the `google-generativeai` Python library to interact with Google's Gemini models. This library requires an **API Key** for authentication, typically obtained via Google AI Studio. Additionally, other credentials like your Google Cloud Service Account key and GCS bucket name need to be configured securely, especially when deploying.

**1. Obtain Your Gemini API Key:**

* **Visit Google AI Studio:** Navigate to [aistudio.google.com](https://aistudio.google.com/).
* **Sign In:** Use a Google Account (personal or Google Workspace).
* **Get API Key:** Find the "**Get API key**" option (usually in the left menu).
* **Create API Key:** Click "**Create API key**" within a new or existing AI Studio project.
* **Copy Your Key:** Copy the generated API key string immediately.

<p align="center">
  <a href="https://console.cloud.google.com/apis/credentials">
    <img src="image\image-70.png"" alt="Example API Key Generation in Google AI Studio" width="600">
  </a>
  <br/><em>(Illustrative example of API key generation interface in Google AI Studio)</em>
</p>


**Important Security Note:**

* Treat the API key like a password. **Do not commit it to Git.**
* Anyone with this key can make API calls billed to your account/project.

**2. Configure Your Application Environment:**

You need to provide the Gemini API key, your Google Cloud Service Account credentials, and your GCS bucket name to the application. How you do this depends on your environment (local development vs. deployment).

* **Local Development (`.env` file):**
    * Create a `.env` file in your project root (add it to `.gitignore`).
    * Add your keys and settings:
        ```dotenv
        # .env file for LOCAL DEVELOPMENT ONLY
        GOOGLE_API_KEY ="YOUR GEMINI API KEY"
        GOOGLE_APPLICATION_CREDENTIALS="/path/to/your/downloaded-service-account.json" # Use the PATH here
        GCS_BUCKET_NAME="your-gcs-bucket-name"
        ```
    * Use a library like `python-dotenv` (`pip install python-dotenv`) in your Python code to load these variables locally:
        ```python
        from dotenv import load_dotenv
        load_dotenv()
        import os
        GOOGLE_API_KEY = os.getenv("GOOGLE_API_KEY")
        ```

* **Heroku Deployment (Config Vars):**
    * Heroku **does not use `.env` files** in the deployed environment. You **must** use **Config Vars**.
    * Set Config Vars via the Heroku CLI or the Heroku Dashboard (under your app's "Settings" tab -> "Config Vars").

    * **Using Heroku CLI:**
        ```bash
        # Replace 'petrodoc-destiny' with your actual Heroku app name

        # Set the Gemini API Key
        heroku config:set GOOGLE_API_KEY = os.environ.get("GOOGLE_API_KEY") -a your-heroku-app-name

        # Set the GCS Bucket Name
        heroku config:set GCS_BUCKET_NAME="your-gcs-bucket-name" -a your-heroku-app-name

        # Set the Google Service Account Credentials
        Replace the service_credentials.json files with the new one, should remain on the root directory.
        ```

**3. Accessing Variables in Code:**

Ensure your Python code (e.g., inside `FileParser.__init__`, `OilGasDocumentAnalyzer.__init__`, or Django `settings.py`) reads these values from the environment using `os.environ.get('VARIABLE_NAME')` or `os.getenv('VARIABLE_NAME')`.


4. **Install Dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

### Dependency Management

The requirements.txt file lists all Python dependencies for this project. Ensure system packages (poppler, etc.) are installed separately as per the prerequisites.

---

## Configuration Deep Dive

Understand the key parameters for customizing behavior:

* **Environment Variables (Priority):**
  * `GOOGLE_API_KEY`: **Required.** For authenticating with the Gemini API.
  * `GOOGLE_APPLICATION_CREDENTIALS`: **Required.** Standard way to provide the *path* to the service account JSON key.
  * `GCS_BUCKET_NAME`: **Required.** The name of the Google Cloud Storage bucket for Vision API operations.

* **FileParser (__init__) Parameters:**
  * `cache_dir` (str): Filesystem path for caching parsed results (Default: `.cache`). *Not recommended for production/Heroku.*
  * `service_account_file` (str | dict): Overrides environment variables if provided directly. Accepts path or credential dictionary.
  * `gcs_bucket_name` (str): Overrides environment variable if provided directly.
  * `token_path` (str), `use_service_account` (bool): For configuring OAuth flow instead of service accounts (mainly for Drive/Docs).

* **OilGasDocumentAnalyzer (__init__) Parameters:**
  * `google_api_key` (str): Overrides environment variable if provided directly.
  * `model_name` (str): Specify the Gemini model ID ("gemini-2.0-flash" was used by default).
  * `cache_dir` (str): Filesystem path for caching analysis results (Default: `.cache`). *Not recommended for production/Heroku.*
  * `service_account_file` (str | dict): needed by underlying Langchain Google components. Pass the same credentials used by FileParser.
  * `well_similarity_threshold_1/_2` (float): Tune well name verification strictness (Defaults: 0.6, 0.3).

---

## Usage Guide

Example script demonstrating parsing a file and analyzing its content:

```python
import os
import json
import traceback
from dotenv import load_dotenv

from file_parser import FileParser
from analyze import OilGasDocumentAnalyzer

# Load environment variables from .env file (if using one)
load_dotenv()

# --- Configuration & Data Loading ---
GOOGLE_API_KEY = os.getenv("GEMINI_API_KEY")
# Prioritize content env var, then path env var
SERVICE_ACCOUNT_CRED = os.getenv("GOOGLE_APPLICATION_CREDENTIALS")
GCS_BUCKET_NAME = os.getenv("GCS_BUCKET_NAME")
CACHE_DIR = ".cache_dev" # Use a separate cache for dev if needed
FILE_TO_ANALYZE = "/path/to/your/document.pdf" # <--- CHANGE THIS TO ANY OF THE SUPPORTED FILE TYPES

def load_json_data(filepath, default=[]):
    """Safely loads JSON data from a file."""
    try:
        with open(filepath, 'r', encoding='utf-8') as f:
            return json.load(f)
    except FileNotFoundError:
        print(f"Info: Data file '{filepath}' not found. Using default: {default}")
        return default
    except json.JSONDecodeError as e:
        print(f"Warning: Error decoding JSON from '{filepath}': {e}. Using default: {default}")
        return default

# Load categories and well/field database from JSON files
CATEGORIES = load_json_data('categories.json', default=[{'name': 'Default', 'description': 'Default category'}])
WELL_FIELD_DATABASE = load_json_data('well_field_db.json', default=[]) # Vital for verification

# --- Validate Essential Configuration ---
if not all([GOOGLE_API_KEY, SERVICE_ACCOUNT_CRED, GCS_BUCKET_NAME]):
    raise ValueError("Missing required configuration: Check GEMINI_API_KEY, GOOGLE_APPLICATION_CREDENTIALS/CONTENT, GCS_BUCKET_NAME")
# Add more robust check depending on whether CRED is path or content
if not os.path.exists(FILE_TO_ANALYZE):
     raise FileNotFoundError(f"Input file not found: {FILE_TO_ANALYZE}")

# --- Initialization ---
print("Initializing File Parser...")
# NOTE: Assumes FileParser.__init__ is adapted to handle SERVICE_ACCOUNT_CRED being path OR content dict
file_parser = FileParser(
    cache_dir=CACHE_DIR,
    service_account_file=SERVICE_ACCOUNT_CRED,
    gcs_bucket_name=GCS_BUCKET_NAME
)

print("Initializing Document Analyzer...")
analyzer = OilGasDocumentAnalyzer(
    google_api_key=GOOGLE_API_KEY,
    cache_dir=CACHE_DIR,
    service_account_file=SERVICE_ACCOUNT_CRED
)

print("Initializing Analyzer knowledge bases...")
try:
    analyzer.initialize_categories(CATEGORIES, debug=False) # Set debug=True for verbose init
    analyzer.initialize_well_field_database(WELL_FIELD_DATABASE, debug=False)
except Exception as e:
    print(f"FATAL: Error initializing analyzer knowledge bases: {e}")
    exit(1)

# --- Processing ---
print(f"\nProcessing file: {FILE_TO_ANALYZE}...")
try:
    # 1. Parse the file (set debug=True for verbose logs)
    extracted_text = file_parser.parse_file(FILE_TO_ANALYZE, max_pages=5, debug=False)

    if extracted_text is None or (isinstance(extracted_text, str) and not extracted_text.strip()):
        print("Parsing failed or yielded no text content.")
    elif isinstance(extracted_text, dict) and "error" in extracted_text:
        print(f"Parsing Error: {extracted_text['error']}")
    else:
        print(f"Parsing successful. Analyzing extracted text (~{len(extracted_text)} chars)...")
        # 2. Analyze text (set debug=True for verbose logs)
        analysis_result = analyzer.analyze_text(extracted_text, debug=False)

        # 3. Display results
        print("\n--- Analysis Results (JSON) ---")
        print(json.dumps(analysis_result, indent=2))

except ValueError as e:
    print(f"Processing Error: Unsupported file type? - {e}")
except FileNotFoundError as e:
    print(f"Processing Error: File not found - {e}")
except Exception as e:
    print(f"FATAL: An unexpected error occurred: {e}")
    traceback.print_exc()
```

---

## Processing Pipeline

### Logical Flow for File Parser (`file_parser.py`)
---

This section outlines the logical processing pipeline within the `FileParser` class, detailing how it handles various file types to extract text or relevant metadata.

1.  **Initialization (`__init__`):**
    * Sets up configuration parameters provided during instantiation: `cache_dir`, `service_account_file` (or dict/env var), `token_path` (for OAuth), `use_service_account` flag, `gcs_bucket_name`, and `max_concurrent_ocr` (less relevant for current cloud-heavy approach).
    * Creates the cache directory if it doesn't exist.
    * Initializes Google API clients:
        * `_initialize_google_api`: Sets up `drive_service` and `docs_service` using specified credentials (Service Account preferred, OAuth fallback).
        * `_initialize_gcs_vision`: Sets up `storage_client` and `vision_client` using Service Account credentials. Ensures the target GCS bucket exists, creating it if necessary.
    * Handles potential errors during API client initialization.

2.  **Main Dispatch (`parse_file`):**
    * Acts as the primary entry point, taking a `file_path` and optional `max_pages`, `debug` arguments.
    * Determines the file type by inspecting the file extension (case-insensitive).
    * Uses an internal dictionary mapping extensions (e.g., `.pdf`, `.docx`, `.las`) to the corresponding specialized parsing method within the class.
    * Calls the matched specialized method to handle the specific file type.
    * Raises a `ValueError` if the file extension is not found in the supported types map.

3.  **Caching Check (Common Entry):**
    * Most specialized parsing methods (`parse_pdf`, `parse_doc`, `parse_las`, etc.) begin by calculating an MD5 hash of the input file content (`_generate_file_hash`).
    * A unique cache file path is generated using this hash and potentially parsing parameters (`_get_cache_path`).
    * It checks if a file exists at this cache path. If yes, it loads the cached result using `pickle.load` and returns it immediately, skipping further processing for that file.

4.  **Core Parsing Execution (Type-Specific):**
    * **PDF (`parse_pdf`) / TIFF (`parse_tiff`):**
        * Uploads the file to the configured Google Cloud Storage (GCS) bucket.
        * Submits an asynchronous job to the **Google Cloud Vision API** (`DOCUMENT_TEXT_DETECTION` feature).
        * Waits for the Vision API job to complete (includes timeout and retry logic).
        * Downloads the JSON results produced by the Vision API from GCS.
        * Parses the JSON to extract the full detected text.
        * Cleans up the temporary files uploaded to and generated within GCS.
    * **DOC/DOCX (`parse_doc`):**
        * *Attempt 1:* Tries local text extraction using `textract`'s default method.
        * *Attempt 2:* If the file is `.doc` and Attempt 1 yielded minimal text, tries `textract` again, explicitly using the `antiword` method (requires `antiword` tool installed).
        * *Attempt 3 (Cloud Fallback):* If local methods fail, initiates a **Google Drive API Conversion Flow**:
            1.  Uploads DOC/DOCX to Drive (`_upload_file_to_drive`).
            2.  Converts the uploaded file to native Google Docs format (`_convert_to_google_docs`).
            3.  (Optionally) Attempts to truncate the Google Doc to approximately `max_pages` using `_limit_pages` (based on paragraph estimation).
            4.  Exports the (potentially truncated) Google Doc to PDF format (`_export_to_pdf`).
            5.  Saves the exported PDF content to a temporary local file.
            6.  Calls `self.parse_pdf` (triggering the Vision API flow described above) on this temporary PDF.
            7.  Cleans up the temporary files on Google Drive and the local temporary PDF (`_cleanup_drive_file`).
        * Applies final text limiting via `_limit_text`.
    * **PPT/PPTX (`parse_ppt`):**
        * *Attempt 1 (Cloud Conversion):* Initiates a **Google Drive API Conversion Flow**: Upload -> Convert to Google Slides -> Export as PDF -> `self.parse_pdf` (Vision API) -> Cleanup Drive/Temp files.
        * *Attempt 2 (Local Fallback):* If cloud conversion fails or yields no text:
            1.  If `.ppt`, tries using the `catppt` command-line tool via `subprocess`.
            2.  If `.pptx` (or `catppt` failed), tries using the `python-pptx` library directly to extract text from shapes.
        * Applies hardcoded text limiting (`[:1000]`).
    * **XLS (`parse_old_xls`):**
        * *Attempt 1 (Direct Read):* Tries parsing directly using `pandas.read_excel` with the `xlrd` engine. Includes logic for handling multiple sheets, sampling large sheets, and basic cleaning (`_try_direct_xls_read`).
        * *Attempt 2 (Cloud Conversion):* If direct read fails, uses **Google Drive API Conversion Flow**: Upload -> Export *directly* as CSV -> Save temporary CSV -> Parse using `self.parse_csv` -> Cleanup Drive/Temp files (`_try_google_drive_conversion`).
    * **XLSX (`parse_xlsx`):**
        * Parses directly using `pandas.read_excel` (with `openpyxl` engine).
        * Reads all sheets, concatenates data, performs aggressive cleaning (removing numbers, dots, NaNs, specific text like "Unnamed:"), samples to first 50 rows during string conversion, and returns the resulting string.
    * **LAS (`parse_las`):** Uses `lasio` library to read header sections and curve information, formatting the metadata and sample curve data into a descriptive text string.
    * **DLIS (`parse_dlis_file`):** Uses `dlisio` library to load the file, extracts metadata (well name, field name, frames, etc.) and samples channel/parameter names, formatting results into a descriptive text string.
    * **SEGY (`parse_segy`):** Uses `segyio` library to open the file, extracts the textual header and binary header keys, attempts to get trace header keys, and formats this information into a text string. Includes handling for files without trace data.
    * **PDS (`parse_pds`):** Reads the file in binary mode, extracts printable ASCII characters, uses specific regex patterns (`#WN.`, `#FN.`) to find well/field names, performs custom keyword removal and token de-duplication, formats results into a text string.
    * **CSV (`parse_csv`):** Uses `pandas.read_csv` with robust features: attempts multiple encodings (`utf-8`, `latin-1`, etc.), auto-detects delimiters (using character counts and `csv.Sniffer`), samples large files intelligently, cleans data, and converts relevant content to a string representation.
    * **TXT (`parse_txt`):** Uses standard Python file I/O with specified encoding. Performs line-level cleaning (stripping whitespace, quotes, tabs, removing decorative lines).

5.  **Caching Result (Common Exit):**
    * If the parsing logic in Step 4 successfully produces a result (typically a string), it is serialized using `pickle.dump` and saved to the cache file path generated in Step 3.

6.  **Return Value:**
    * The specialized parser method returns the extracted text (usually `str`), potentially structured data formatted as text, `None` on some failures, or an error dictionary (`{'error': ...}`) in specific cases. This value is then returned by the main `parse_file` method.

This flow emphasizes the module's role as a dispatcher that leverages caching and employs diverse strategies, including sophisticated cloud services and specific libraries, tailored to each supported file format.

### Logical Flow for `analyze.py`
---

This section outlines the logical flow of the `OilGasDocumentAnalyzer`

1.  **Input & Caching:** The process begins with input text (either provided directly or extracted from a file via `FileParser`). A file-based cache (`.cache` directory) is checked first using a hash of the input text or file. If a valid cached result exists, it's returned immediately.

2.  **Initial LLM Extraction (`_unified_extraction`):** If not cached, the first ~5000 characters of the text are sent to the configured Gemini LLM. The model is prompted to return a JSON object containing lists of `concepts`, `well_names`, `primary_wells`, and `field_names`. Well names and concepts undergo initial normalization at this stage.

3.  **Well Name Verification (`_process_extracted_well_names`):**
    * This function is called separately for both `well_names` and `primary_wells` obtained from the LLM.
    * It extracts the **prefix** from each potential well name.
    * This prefix is used to perform a vector **similarity search** against the `well_field_store` (ChromaDB).
    * Based on similarity score thresholds (e.g., 0.8, 0.2), the original prefix might be kept, replaced by a matching prefix from the database, or the entire well name might be discarded if confidence is too low or components are missing.
    * Returns lists of verified (and potentially corrected) full well names.

4.  **Field Name Verification - Attempt 1 (`_process_field_names`):**
    * Receives the raw `field_names` list from the LLM and the list of *full* verified primary well names.
    * **Scenario A (LLM provided fields):** The raw field names are cleaned. Each cleaned name is compared against **all** field names present in the `well_field_store` metadata using **string similarity** (Levenshtein distance). Based on thresholds (e.g., 0.7, 0.1, hard-coded) against the best global match, the LLM-extracted field is kept, replaced by the DB match, or discarded.
    * **Scenario B (LLM provided no fields):** The function calls `_lookup_fields_from_database`, passing the list of **full** verified primary well names to attempt finding associated fields directly from the database metadata.

5.  **Document Categorization (`_categorize_with_rag`, `_categorize_with_gemini`, `_combine_categorization_results`):**
    * **RAG:** Extracted `concepts` are used to perform a vector similarity search against predefined categories and example documents (in ChromaDB stores).
    * **LLM:** Truncated text (~5000 chars) and the list of possible categories are sent to Gemini for direct classification, confidence scoring, and reasoning.
    * **Combination:** Results from RAG and LLM are combined using a weighting logic that prioritizes high-confidence Gemini results but also considers agreement between the two methods.

6.  **Field Name Verification - Attempt 2 (Fallback in `analyze_text`):**
    * After the initial field verification (Step 4), the code checks if the resulting list of field names is effectively empty.
    * If it *is* empty, it performs a **second, distinct database lookup**. It extracts the **prefix** from only the *first* verified primary well name and calls `_lookup_fields_from_database` with just this single prefix. If this lookup succeeds, its results *overwrite* the previously empty field list.

7.  **Final Output Generation & Caching:**
    * Verified well names are formatted (uppercase, hyphen before first digit via `_format_well_name`).
    * Verified field names are formatted (title case).
    * A short summary (5-30 words) is generated by sending truncated text (~5000 chars) to Gemini (`_generate_summary`).
    * All derived information (category, confidence, reasoning, formatted wells, formatted fields, concepts, summary, etc.) is compiled into a final dictionary.
    * This dictionary is saved to the file-based cache.
    * The final dictionary is returned.

This original pipeline involved multiple stages of verification and fallback, particularly for field names, with distinct logic relying on prefix-based vector search for wells vs. global string similarity or full-name lookups for fields, plus a final prefix-based fallback.

### Data Flow
---

**Recommended Document Upload Flow:**
   - User selects document in React frontend
   - Frontend uploads document via API to Django backend
   - Django creates Document model instance
   - Django dispatches Celery task for processing
   - Celery worker runs FileParser and OilGasDocumentAnalyzer in background
   - Worker updates Document status and creates DocumentAnalysis
   - Frontend polls document status and shows results when complete


---

## Deployment Guide (Heroku)

### Procfile Configuration

Create a `Procfile` in your project root with both web and worker processes:

```
web: gunicorn your_project.wsgi:application --log-file -
worker: celery -A your_project worker -l INFO
```

### Dependencies (System & Python)

1. **Create an Aptfile** for system dependencies:
   ```
   poppler-utils
   antiword
   catdoc
   libgl1-mesa-glx
   ```

2. **requirements.txt** should include:
   ```
   
   # Storage & Media
   boto3>=1.20.0  # For AWS S3
   
   # PetroDoc Core Dependencies
   google-generativeai>=0.1.0
   langchain>=0.0.200
   langchain-google-genai>=0.0.5
   chromadb>=0.4.0
   google-api-python-client>=2.70.0
   google-cloud-vision>=3.4.0
   google-cloud-storage>=2.7.0
   pandas>=1.5.0
   lasio>=0.30.0
   dlisio>=0.4.0
   segyio>=1.9.0
   python-pptx>=0.6.0
   textract>=1.6.0
   Pillow>=9.0.0
   pdf2image>=1.16.0
   PyMuPDF>=1.21.0  # For PDF parsing
   python-dotenv>=0.20.0
  
   ```

### Configuration Variables

Set the following environment variables in Heroku dashboard:

1. **Essential**:
   * `SECRET_KEY`: Django secret key
   * `GEMINI_API_KEY`: Google Gemini API key
   * `GOOGLE_APPLICATION_CREDENTIALS_CONTENT`: Base64-encoded GCP service account JSON
   * `GCS_BUCKET_NAME`: Google Cloud Storage bucket name
   * `AWS_ACCESS_KEY_ID`: For S3 storage
   * `AWS_SECRET_ACCESS_KEY`: For S3 storage
   * `AWS_STORAGE_BUCKET_NAME`: For S3 storage

2. **Recommended**:
   * `DEBUG`: Set to "False" for production
   * `ALLOWED_HOSTS`: Domain names allowed to serve the application


### Buildpacks & Aptfile

1. **Add buildpacks** (in this order):
   * heroku-community/apt (for system dependencies)
   * heroku/python (for Python dependencies)

2. **Create Aptfile** in project root with required system dependencies:
   ```
   poppler-utils
   antiword
   catdoc
   libgl1-mesa-glx
   ```

### Limitations & Considerations

1. **Ephemeral Filesystem**:
   * Heroku's filesystem is ephemeral - files written to disk don't persist between dynos restarts
   * Use AWS S3 or Google Cloud Storage for persistent file storage
   * Configure Django to use S3 backend for media files

2. **Dyno Memory**:
   * Processing large documents may exceed standard dyno memory limits
   * Consider upgrading to Performance-M or higher dynos for worker processes
   * Implement chunking strategies for very large documents

3. **Request Timeout**:
   * Heroku has a 30-second timeout for web requests
   * Ensure all document processing is handled by background workers
   * Implement status polling and result caching

4. **(recommended)Add-ons Required**:
   * Heroku PostgreSQL (database)
   * Heroku Redis (caching and Celery broker)
   * Consider Papertrail or LogDNA for log management

---

## Model Costs Analysis (Estimates)

This section provides a cost analysis for the cloud-based AI services potentially used by the PetroDoc AI (`FileParser` and `OilGasDocumentAnalyzer`).

**🚨 IMPORTANT DISCLAIMERS 🚨**

* **Estimate Only:** This is a **rough estimate** based on numerous assumptions about API usage and publicly available pricing. **Actual costs WILL vary.**
* **Pricing Volatility:** Cloud provider pricing changes. These estimates use pricing data inferred around **mid-April 2025**. Always consult the official pricing pages for Google Cloud (Vertex AI, Document AI, Vision AI, Storage, Drive) for current rates in your region.
* **Infrastructure Costs Excluded:** This analysis focuses *only* on potential per-use API costs. It does **not** include costs for hosting (e.g., Heroku dynos), databases (e.g., PostgreSQL for Django, managed ChromaDB if applicable), Redis cache, GCS/S3 storage fees.

**1. Approach: File Type Categorization**

For purpose of costs estimation, we categorize files based on their likely API consumption during parsing and analysis:

* **Type I: Direct Parse / No External API:**
    * *Examples:* `.txt`, `.csv`, `.las`, `.dlis`, `.segy`, `.pds`, `.xlsx` (standard).
    * *API Usage:* No significant API calls expected during *parsing*. Analysis (LLM + Embedding) is performed on the extracted text.
* **Type II: API Dependent (OCR Intensive):**
    * *Examples:* Scanned `.pdf`, `.tif`, `.tiff`. Any PDF where text extraction requires OCR.
    * *API Usage:* **Requires** cloud OCR (e.g., Google Vision / Document AI) for nearly every page. Analysis (LLM + Embedding) follows on the OCR'd text.
* **Type III: Potential API Fallback (Conversion/OCR):**
    * *Examples:* `.doc`, `.docx`, `.ppt`, `.pptx`, `.xls`. Text-based `.pdf` where local extraction might fail.
    * *API Usage:* Attempts local/direct parsing first. If that fails or yields poor results, **may fall back** to cloud APIs (e.g., Google Drive/Docs conversion followed by OCR via `parse_pdf`). API cost is incurred *only* if the fallback is triggered. Analysis (LLM + Embedding) occurs regardless.

**2. Scenario Assumptions**

* **Services Used:** Google Cloud Platform
    * OCR: Document AI OCR Processor
    * LLM: Vertex AI `gemini-2.0-flash` (Chosen for cost-effectiveness vs. Pro)
    * Embedding: Vertex AI Text Embedding ( `text-embedding-004`)
* **Usage Per Page / File (Averages):**
    * Avg. Characters per Page (for analysis): 2,500 (~500-600 words)
    * Avg. LLM Output Characters per Page (summary/extraction): 500
    * Type I Analysis Input: Capped at first 2,500 characters total.
    * Type II Avg. Pages: 7 pages
    * Type III Avg. Pages (if fallback needed): 4 pages
* **Fallback Probability (Type III):** Assume **20%** of Type III files require the cloud conversion/OCR fallback.
* **File Distribution (Example Batch):** 1000 files total
    * Type I: 300 files (30%)
    * Type II: 500 files (50%)
    * Type III: 200 files (20%)
* **Assumed Pricing (Approx. mid-April 2025 - CHECK CURRENT):**
    * Document AI OCR: $0.0015 per page
    * Vertex AI Gemini 2.0 Flash Input: $0.00001875 per 1k characters
    * Vertex AI Gemini 2.0 Flash Output: $0.000075 per 1k characters
    * Vertex AI Embedding Input: $0.000025 per 1k characters (Estimate)

**3. Cost Calculation Breakdown**

* **Cost per API-Intensive Page** (Used in Type II & Type III Fallback):
    * OCR: $0.0015
    * Embedding (2.5k chars): (2.5 * $0.000025) = $0.0000625
    * LLM Flash (2.5k in, 0.5k out): (2.5 * $0.00001875) + (0.5 * $0.000075) = $0.000046875 + $0.0000375 = $0.000084375
    * *Total per Page (OCR + Emb + LLM):* ~$0.001647

* **Cost per File Type:**
    * **Type I File:** (No OCR/Conversion)
        * Embedding (2.5k chars) + LLM Flash (2.5k in, 0.5k out)
        * $0.0000625 + $0.000084375 = **~$0.00015**
        * A thousand of Type I files costs 1000 * 0.00015 = **~$0.15** *
    * **Type II File:** (API cost for all 7 pages)
        * 7 pages * $0.001647/page = **~$0.01153**
        * A thousand of Type II files costs 1000 * 0.01153 = **~$11.53** *
    * **Type III File (Average Cost):**
        * Cost if Fallback (4 pages API intensive): 4 * $0.001647 = $0.006588
        * Cost if No Fallback (Assume similar analysis to Type I): $0.00015
        * Average = (20% * $0.006588) + (80% * $0.00015)
        * Average = $0.0013176 + $0.00012 = **~$0.00144**
        * A thousand of Type III files costs 1000 * 0.00144 = **~$1.44** *

* **Total Estimated Cost for 1000 Files (Example Distribution):**
    * Type I Cost: 300 files * $0.00015/file = $0.045
    * Type II Cost: 500 files * $0.01153/file = $5.765
    * Type III Cost: 200 files * $0.00144/file = $0.288
    * **Total Estimated API Cost:** $0.045 + $5.765 + $0.288 = **~$6.10**

**4. Summary Table (Hypothetical Estimate)**

| Item                       | Est. API Cost (USD) | Key Assumptions (Mid-April 2025 Pricing Estimates)                                     |
| :------------------------- | :------------------ | :------------------------------------------------------------------------------------- |
| **Per Page (API Calls)** | ~$0.00165           | 1 OCR Call, 1 Embedding Call (2.5k chars), 1 Gemini Flash Call (2.5k in, 0.5k out)     |
| **Per File (Type I)** | ~$0.00015           | No OCR/Conversion cost. Analysis (Emb+LLM) on ~2.5k chars total.                    |
| **Per File (Type II)** | ~$0.0115            | Avg. 7 pages per file, all pages incur API costs (OCR+Emb+LLM).                      |
| **Per File (Type III)** | ~$0.00144           | Avg. cost assuming 20% require fallback (4 pages API intensive), 80% don't (Type I cost). |
| **1,000 Files (Example Mix)** | **~$6.10** | 300 Type I, 500 Type II, 200 Type III files.                                           |

**5. Conclusion and Required Actions**

This estimate, using the cost-effective Gemini 2.0 Flash model, suggests that API costs might be relatively low under these specific usage patterns and file type distributions. However, costs could increase significantly if:
* A more powerful (and expensive) LLM like Gemini-2.5 Pro was used.
* More API calls are made per page
* Average document page count is higher.
* Character/token counts per page are higher.
* A larger proportion of files fall into Type II or require Type III fallbacks.

**Check Official Pricing:** Use the official Google Cloud Pricing documentation and calculators for the precise services and regions you deploy to. Factor in potential free tiers or committed use discounts.
 * **Google Vision OCR Pricing:** [cloud.google.com/document-ai/pricing](https://cloud.google.com/vision/pricing)
 * **Vertex AI Pricing (GenAI - Covers Gemini & Embeddings):** [cloud.google.com/vertex-ai/generative-ai/pricing](https://cloud.google.com/vertex-ai/generative-ai/pricing)
 * **Gemini API Pricing (Consumer/AI Studio Key):** [ai.google.dev/gemini-api/docs/pricing](https://ai.google.dev/gemini-api/docs/pricing)

---

## Critical Recommendations & Best Practices

 ### Asynchronous Processing

1. **Implement Celery Workers**:
   * Process all document parsing and analysis asynchronously
   * Configure multiple queues for different priority tasks:
     ```python
     # settings.py
     CELERY_TASK_ROUTES = {
         'documents.tasks.process_document': {'queue': 'document_processing'},
         'documents.tasks.update_vector_db': {'queue': 'maintenance'}
     }
     ```

2. **Progress Tracking**:
   * Implement a progress tracking mechanism for long-running tasks
   * Consider using Celery's task_success signal to update processing statuses

3. **Error Handling & Retries**:
   * Implement exponential backoff for retries
   * Store detailed error information for debugging

### Scalable File Storage

1. **Use S3 for Document Storage**:
   ```python
   # settings.py
   DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
   AWS_S3_ACCESS_KEY_ID = os.environ.get('AWS_ACCESS_KEY_ID')
   AWS_S3_SECRET_ACCESS_KEY = os.environ.get('AWS_SECRET_ACCESS_KEY')
   AWS_STORAGE_BUCKET_NAME = os.environ.get('AWS_STORAGE_BUCKET_NAME')
   AWS_S3_REGION_NAME = os.environ.get('AWS_S3_REGION_NAME', 'us-east-1')
   AWS_S3_SIGNATURE_VERSION = 's3v4'
   AWS_S3_FILE_OVERWRITE = False
   AWS_DEFAULT_ACL = 'private'
   AWS_S3_CUSTOM_DOMAIN = None
   ```

2. **Implement Pre-signed URLs**:
   * Generate pre-signed URLs for secure, time-limited access to documents
   * Avoid exposing document URLs in the frontend

### Configuration Management

1. **Environment-Specific Settings**:
   * Use different settings modules for dev/staging/prod
   * Implement feature flags for gradual rollout

2. **Secret Management**:
   * Never commit secrets to version control
   * Use environment variables or a secrets manager

### Security Considerations

1. **API Key Rotation**:
   * Implement regular rotation of GCP service account keys
   * Use a key management service when possible

2. **Input Validation**:
   * Validate file types and sizes before processing
   * Scan uploaded files for malware (consider ClamAV integration)

3. **Rate Limiting**:
   * Implement API rate limiting to prevent abuse
   * Consider per-user quotas based on subscription level


### Database Optimization

1. **Index Key Fields**:
   * Add indexes for fields used in filtering and sorting
   * Consider partial indexes for status fields

2. **Handle Large Text Fields**:
   * For PostgreSQL, consider using Text Search Configurations for extracted_text
   * Implement pagination for long text display

### Resource Management

1. **Optimize Memory Usage**:
   * Implement chunking for large documents
   * Close file handles properly with context managers
   * Monitor memory usage in worker processes

2. **Worker Concurrency**:
   * Configure appropriate concurrency for Celery workers
   * Use prefork pool for memory-intensive tasks

---

## Troubleshooting

Common issues and solutions:

1. **Vision API Errors**:
   * Ensure proper IAM permissions on the service account
   * Check GCS bucket permissions and lifecycle policies
   * Verify file size limits (Vision API has a 20MB limit per image)

2. **Gemini API Errors**:
   * Check API key and quotas
   * Verify that request size is within limits
   * Handle rate limiting with appropriate backoff strategy

3. **Office Document Conversion Issues**:
   * Ensure the necessary system libraries are installed
   * Check the fallback chain configuration
   * Consider preprocessing complex documents

4. **Memory Issues in Workers**:
   * Implement chunking strategies
   * Increase worker memory allocation
   * Add monitoring to identify problematic documents

5. **OCR Quality Problems**:
   * Adjust preprocessing (contrast, noise reduction)
   * For very poor quality scans, consider trying multiple OCR services
   * Implement manual review workflow for critical documents

---

## Contributing

Guidelines for contributing to PetroDoc AI:

1. **Development Setup**:
   * Fork the repository
   * Set up development environment following Installation & Setup
   * Create feature branches for new functionality

2. **Code Standards**:
   * Follow PEP 8 style guide
   * Include docstrings and type hints


---
##
## License

Copyright © 2025 · Right-Click Solutions · All Rights Reserved
