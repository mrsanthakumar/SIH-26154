# 🔐 ContentForge - AI Content Transformer

> **PS 26154 – Gen AI Platform for Automated Content Transformation**

ContentForge is a Streamlit-based AI application that transforms the content of uploaded PDF documents using generative AI. It combines document processing, multilingual translation, text-to-speech, security screening, document integrity verification, and a blockchain-style audit trail in a single web application.

The application uses the Groq API for AI-powered content generation and provides multiple transformation options such as summarization, bullet-point conversion, entity extraction, risk detection, action-item extraction, simplified explanations, and FAQ generation.

---

## 🚀 Features

### 🤖 AI-Powered Content Transformation

Upload a PDF and select one or more AI transformations:

- **Summarize** – Generates a concise summary of the document.
- **Translate** – Translates document content into:
  - Tamil
  - Hindi
  - Telugu
  - Kannada
  - Malayalam
  - English
  - French
- **Convert to Bullet Points** – Converts document content into clear bullet points.
- **Key Entity Extraction** – Extracts names, organizations, dates, and locations.
- **Risk & Sensitive Content Flagger** – Identifies potentially sensitive, confidential, or risky information.
- **Action Item Extraction** – Extracts tasks and to-do items from the document.
- **Simplify / ELI5** – Explains the document in simple language.
- **FAQ Generation** – Generates five frequently asked questions with short answers.

### 🛡️ Cybersecurity Features

ContentForge includes a security layer for uploaded documents.

#### SHA-256 Document Integrity

A SHA-256 hash is generated from the uploaded PDF. This provides a cryptographic fingerprint that can be used to verify whether the document has changed.

#### PII Detection

The application checks extracted PDF text for common patterns including:

- Email addresses
- Phone numbers
- Credit/debit card-like numbers
- Aadhaar-like numbers
- IP addresses
- Dates of birth

The application produces a rule-based **Security Score**, **Risk Level**, and count of detected PII categories.

> **Note:** The security score is a rule-based screening indicator and should not be treated as a guarantee that a document contains no sensitive information.

### ⛓️ Blockchain-Style Audit Trail

Every AI transformation is recorded in a session-based, hash-linked audit trail.

Each audit block contains:

- Block index
- Timestamp
- Action
- Output hash
- Previous block hash
- Current block hash

The application can verify the links between blocks and report whether the audit chain is intact.

> This is a blockchain-style audit mechanism implemented inside the application; it is not a distributed blockchain network.

### 🔊 Multilingual Text-to-Speech

Translated content can be converted into MP3 audio using Google Text-to-Speech (`gTTS`).

### 📄 Word Document Export

All transformation results recorded during the current session can be exported as a Word document:

`ContentForge_All_Results.docx`

### 📜 Session History

The application maintains a session history of:

- Uploaded filename
- Transformation performed
- Timestamp
- Generated output

Individual history entries can be deleted, or the complete history can be cleared.

---

## 🏗️ System Architecture

```text
                    ┌───────────────────────┐
                    │       User            │
                    │   Upload PDF          │
                    └───────────┬───────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │   Streamlit Web UI    │
                    └───────────┬───────────┘
                                │
                 ┌──────────────┴──────────────┐
                 │                             │
                 ▼                             ▼
       ┌──────────────────┐          ┌────────────────────┐
       │ PDF Text         │          │ SHA-256 Integrity  │
       │ Extraction       │          │ Verification       │
       │ PyPDF2           │          └────────────────────┘
       └────────┬─────────┘
                │
                ▼
       ┌──────────────────┐
       │ Security Score   │
       │ PII Detection    │
       │ Risk Assessment  │
       └────────┬─────────┘
                │
                ▼
       ┌────────────────────────────┐
       │ AI Transformation Engine   │
       │ Groq API                   │
       │ GPT-OSS-20B                │
       └────────────┬───────────────┘
                    │
        ┌───────────┼───────────────┐
        ▼           ▼               ▼
   Summary      Translation     Extraction
        │           │               │
        ▼           ▼               ▼
   Simplify       gTTS         Risk / Actions /
   Bullet         Audio        Entities / FAQs
        │
        └──────────────┬──────────────┘
                       ▼
             ┌────────────────────┐
             │ Audit Trail        │
             │ Hash-linked Blocks │
             └─────────┬──────────┘
                       ▼
             ┌────────────────────┐
             │ History & Export   │
             │ Word Document      │
             └────────────────────┘
```

---

## 🔄 Application Workflow

1. User opens the ContentForge Streamlit application.
2. User uploads a PDF document.
3. The application reads the PDF and extracts its text using PyPDF2.
4. A SHA-256 hash is calculated for document integrity.
5. The extracted content is scanned for common PII patterns.
6. A security score and risk level are calculated.
7. User selects one or more transformation operations.
8. The selected content is sent to the Groq AI model.
9. AI-generated results are displayed in the Streamlit interface.
10. Translation results can additionally be converted to speech.
11. Each transformation is recorded in the hash-linked audit trail.
12. Results are stored in the current Streamlit session history.
13. All results can be exported as a Word document.
14. The audit chain can be verified for broken links.

---

## 🧰 Technology Stack

| Component | Technology |
|---|---|
| Frontend / UI | Streamlit |
| Programming Language | Python |
| PDF Processing | PyPDF2 |
| Generative AI | Groq API |
| AI Model | `openai/gpt-oss-20b` |
| Text-to-Speech | gTTS |
| Security Hashing | SHA-256 |
| Document Export | python-docx |
| Text Pattern Detection | Python Regular Expressions |
| Session State | Streamlit Session State |

---

## 📦 Python Dependencies

The application imports the following main packages:

```text
streamlit
PyPDF2
groq
gTTS
python-docx
```

A suitable `requirements.txt` can be created as:

```text
streamlit
PyPDF2
groq
gTTS
python-docx
```

---

## 💻 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR-USERNAME/ContentForge.git
cd ContentForge
```

Replace `YOUR-USERNAME` with your GitHub username and adjust the repository name if required.

### 2. Create a Virtual Environment

#### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

#### macOS / Linux

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

If `requirements.txt` is not available:

```bash
pip install streamlit PyPDF2 groq gTTS python-docx
```

---

## 🔑 Configure the Groq API Key

ContentForge reads the API key from the environment variable:

```text
GROQ_API_KEY
```

### Windows PowerShell

```powershell
$env:GROQ_API_KEY="YOUR_GROQ_API_KEY"
```

### Windows Command Prompt

```cmd
set GROQ_API_KEY=YOUR_GROQ_API_KEY
```

### macOS / Linux

```bash
export GROQ_API_KEY="YOUR_GROQ_API_KEY"
```

Do **not** hard-code your API key inside the Python source code or commit it to GitHub.

---

## ▶️ Run the Application

If your Python file is named `app.py`:

```bash
streamlit run app.py
```

If your file has a different name, for example `contentforge_app.py`:

```bash
streamlit run contentforge_app.py
```

Streamlit will provide a local URL, normally similar to:

```text
http://localhost:8501
```

Open the displayed address in your web browser.

---

## 📖 How to Use

### Step 1 – Upload a PDF

Click:

**Upload a PDF**

and select a PDF document.

### Step 2 – Check Security Information

After uploading, ContentForge displays:

- SHA-256 document hash
- Security Score
- Risk Level
- Number of PII categories detected
- Detected PII patterns

### Step 3 – Select Transformations

Choose the operations you need:

```text
☑ Summarize
☑ Translate
☑ Convert to Bullet Points
☑ Extract Key Entities
☑ Risk & Sensitive Content Flagger
☑ Extract Action Items
☑ Simplify
☑ Generate FAQs
```

### Step 4 – Translate and Generate Voice

When **Translate** is selected, choose one or more supported languages.

The translated result can also be played as audio through the application's text-to-speech feature.

### Step 5 – Transform

Click:

**Transform**

The selected AI operations are executed and the results are displayed.

### Step 6 – Review Audit Trail

The application records transformation outputs in a hash-linked audit chain.

The interface displays:

- Block number
- Timestamp
- Action
- Output hash
- Previous block reference
- Block hash

### Step 7 – Export Results

When results are available, click:

**Download All Results as Word Document**

to create:

```text
ContentForge_All_Results.docx
```

---

## 🔐 Security Design

ContentForge implements several security-oriented mechanisms.

### 1. Document Integrity

```text
PDF Bytes
    │
    ▼
SHA-256
    │
    ▼
Document Fingerprint
```

The hash is calculated directly from the uploaded file bytes.

### 2. PII Pattern Detection

```text
PDF
 │
 ▼
Text Extraction
 │
 ▼
Regular Expression Matching
 │
 ├── Email
 ├── Phone
 ├── Card-like Number
 ├── Aadhaar-like Number
 ├── IP Address
 └── Date of Birth
 │
 ▼
Security Score
```

### 3. Audit Chain

```text
Block 1
   │
   ├── Output Hash
   │
   ▼
Block 2
   │
   ├── Previous Hash
   │
   ▼
Block 3
   │
   └── Previous Hash
```

Each transformation is linked to the previous audit entry using cryptographic hashes.

---

## 📊 Security Score Logic

The application calculates a simple rule-based score.

When no configured PII patterns are detected:

```text
Score = 100
Risk = LOW
```

When findings exist, the score is reduced according to the number of detected occurrences, with the reduction capped by the application's implemented formula.

Risk levels are classified as:

| Score | Risk Level |
|---:|---|
| 70–100 | LOW |
| 40–69 | MEDIUM |
| 0–39 | HIGH |

This mechanism is intended as a screening indicator rather than a complete security or privacy assessment.

---

## 📁 Recommended Project Structure

```text
ContentForge/
│
├── contentforge_app.py
├── requirements.txt
├── README.md
├── .gitignore
└── screenshots/
    ├── dashboard.png
    ├── security-score.png
    ├── transformation-results.png
    └── audit-trail.png
```

---

## 🖼️ Screenshots

Add screenshots of your running application to the `screenshots` folder and update this section.

### Dashboard

```markdown
![ContentForge Dashboard](screenshots/dashboard.png)
```

### Security Score

```markdown
![Security Score Dashboard](screenshots/security-score.png)
```

### Transformation Results

```markdown
![Transformation Results](screenshots/transformation-results.png)
```

### Audit Trail

```markdown
![Blockchain-Style Audit Trail](screenshots/audit-trail.png)
```

---

## ⚠️ Limitations

- The application currently accepts PDF files as the uploaded document format.
- PII detection is based on predefined regular-expression patterns and may produce false positives or miss information that does not match those patterns.
- The security score is a rule-based indicator and is not a complete cybersecurity assessment.
- The audit trail is stored in Streamlit session state and is therefore session-based rather than a persistent distributed blockchain.
- Translation voice generation depends on the availability of the gTTS service.
- AI-generated results depend on the configured Groq model and API availability.
- Large PDF documents may require substantial processing time and API usage.

---

## 🔮 Future Enhancements

Possible future improvements include:

- Persistent database-backed audit logs
- User authentication and role-based access control
- Support for DOCX, TXT, and image documents
- OCR for scanned PDFs
- Advanced Named Entity Recognition
- More comprehensive PII detection
- Configurable security policies
- Encryption at rest and in transit
- Persistent document history
- Downloadable PDF reports
- Advanced analytics dashboard
- Enterprise audit logging
- Local/offline AI model support
- More multilingual voice options

---

## 🎯 Use Cases

ContentForge can be used for:

- Academic document processing
- Research document summarization
- Educational content transformation
- Multilingual document assistance
- Document risk screening
- Sensitive-information awareness
- Automated action-item extraction
- FAQ generation
- Content simplification
- AI-assisted document analysis

---

## 🧪 Example

A user uploads a research paper and selects:

```text
✓ Summarize
✓ Extract Key Entities
✓ Generate FAQs
✓ Risk & Sensitive Content Flagger
```

ContentForge processes the PDF and provides the selected outputs while recording the transformation activity in the audit trail.

---

## 🔒 API Key Security

Never commit secrets such as:

```text
GROQ_API_KEY
```

to GitHub.

Recommended `.gitignore` entries:

```text
venv/
.env
__pycache__/
*.pyc
.DS_Store
```

If using a `.env` file in future development, keep it out of version control:

```text
.env
```

---

## 📌 Project Information

**Project:** ContentForge - AI Content Transformer  
**Problem Statement:** PS 26154  
**Category:** Generative AI / Document Intelligence / Cybersecurity  
**Interface:** Streamlit  
**Language:** Python

---

## 👨‍💻 Author

**Your Name**

Replace this section with your name, institution, GitHub profile, and contact information.

```text
Author: Your Name
Institution: Your Institution
GitHub: https://github.com/YOUR-USERNAME
```

---

## 📄 License

Add your preferred open-source license before publishing the repository.

For example:

```text
MIT License
```

If a different institutional or project license applies, replace this section accordingly.

---

## ⭐ Acknowledgements

- Streamlit for the interactive Python web application framework.
- Groq for AI model API access.
- PyPDF2 for PDF text extraction.
- gTTS for text-to-speech generation.
- python-docx for Word document generation.

---

## 📬 Contribution

Contributions and suggestions are welcome.

A typical contribution workflow is:

```bash
git checkout -b feature/new-feature
git add .
git commit -m "Add new feature"
git push origin feature/new-feature
```

Then create a Pull Request on GitHub.

---

## 📜 Disclaimer

ContentForge is an AI-assisted document transformation and security-screening application. AI-generated content and automated security findings should be reviewed by a human before being used for important academic, legal, financial, medical, or security decisions.
