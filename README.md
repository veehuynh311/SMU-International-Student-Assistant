# 🎓 SMU International Student Assistant

A RAG (Retrieval-Augmented Generation) assistant to help F1 international students at Southern Methodist University (SMU) navigate complex visa regulations, employment authorization, taxes, SSN applications, and Texas driver's license requirements.

## 📋 Problem Statement

**Domain:** F1 Visa Navigation for International Students at SMU

**Target User:** F1 international students at SMU, especially new arrivals and those seeking employment authorization (internships, post-graduation jobs).

**Problem:** International students face a maze of complex regulations across multiple life areas: maintaining F1 status, work authorization (CPT/OPT/STEM OPT), filing taxes, obtaining an SSN, and getting a Texas driver's license. Information is scattered across 6+ government agencies (USCIS, IRS, DHS, SSA, Texas DPS) and university resources. One mistake can jeopardize visa status, leading to serious consequences including deportation. This RAG assistant consolidates 12 official documents into a single conversational interface, providing accurate, source-cited answers.

**Example Questions:**
- "How do I apply for CPT?"
- "Will full-time CPT affect my OPT eligibility?"
- "How do I get an SSN as an F1 student?"
- "What documents do I need for a Texas driver's license?"
- "Do I need to file taxes if I didn't work?"

## 🏗️ Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     SMU International Student Assistant                  │
└─────────────────────────────────────────────────────────────────────────┘

┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Documents │     │  Ingestion  │     │  Chunking   │     │ Embeddings  │
│  (12 files) │────▶│  & Cleaning │────▶│  (600 char) │────▶│  (MiniLM)   │
│  PDF, HTML  │     │  PyPDF, BS4 │     │  LangChain  │     │ sentence-tf │
└─────────────┘     └─────────────┘     └─────────────┘     └──────┬──────┘
                                                                    │
                                                                    ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Gradio    │     │  RAG Chain  │     │   Prompt    │     │ Vector Store│
│     UI      │◀────│   + LLM     │◀────│  Assembly   │◀────│   (FAISS)   │
│             │     │             │     │  + Context  │     │   top-k=5   │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
       │
       ▼
┌─────────────────────────────────────────────────────────────────────────┐
│  Output: Answer + Source Citations (doc name, page) + "I don't know"    │
└─────────────────────────────────────────────────────────────────────────┘

Data Flow:
1. User asks question via Gradio UI
2. Question → Embedding → FAISS similarity search → Top-k chunks
3. Retrieved chunks + Question → Prompt template → LLM
4. LLM generates answer with citations from chunk metadata
5. Display answer + sources to user
```

## 📁 Project Structure

```
smu-international-student-assistant/
├── documents/                           # Source documents (12 files)
│   ├── smu_new_student_info.html
│   ├── smu_current_student_info.html
│   ├── smu_us_living.html
│   ├── uscis_opt.html
│   ├── uscis_stem_opt.html
│   ├── dhs_cpt_guide.html
│   ├── ice_practical_training.html
│   ├── irs_form_8843.html
│   ├── sprintax_tax_guide.html
│   ├── dhs_ssn_guide.html
│   ├── ssa_international_students_ssn.pdf
│   └── texas_dps_dl_checklist.pdf
├── notebooks/
│   └── SMU_International_Student_Assistant.ipynb
├── chunks.json                          # Processed chunks with metadata
├── chunking_stats.json                  # Chunking statistics
├── README.md
└── requirements.txt
```

## 📊 Data Sources (12 Documents)

| # | Source | Topic | Type | Filename |
|---|--------|-------|------|----------|
| 1 | SMU ISSS | New Student Information | HTML | `smu_new_student_info.html` |
| 2 | SMU ISSS | Current Student Information | HTML | `smu_current_student_info.html` |
| 3 | SMU ISSS | US Living (Tax, SSN, DL) | HTML | `smu_us_living.html` |
| 4 | USCIS | OPT for F-1 Students | HTML | `uscis_opt.html` |
| 5 | USCIS | STEM OPT Extension | HTML | `uscis_stem_opt.html` |
| 6 | DHS Study in the States | CPT Guide | HTML | `dhs_cpt_guide.html` |
| 7 | ICE | Practical Training Overview | HTML | `ice_practical_training.html` |
| 8 | IRS | Form 8843 Instructions | HTML | `irs_form_8843.html` |
| 9 | Sprintax | F1 Student Tax Guide | HTML | `sprintax_tax_guide.html` |
| 10 | DHS Study in the States | Obtaining SSN | HTML | `dhs_ssn_guide.html` |
| 11 | SSA | SSN for International Students | PDF | `ssa_international_students_ssn.pdf` |
| 12 | Texas DPS | Driver License Checklist | PDF | `texas_dps_dl_checklist.pdf` |

## 📂 Topic Coverage

| Topic | Document Sources |
|-------|------------------|
| F1 Status & Regulations | #1, #2, #3 (SMU ISSS) |
| CPT (Curricular Practical Training) | #6 (DHS), #7 (ICE) |
| OPT (Optional Practical Training) | #4 (USCIS), #7 (ICE) |
| STEM OPT Extension | #5 (USCIS) |
| Taxes for F1 Students | #3 (SMU), #8 (IRS), #9 (Sprintax) |
| Social Security Number (SSN) | #3 (SMU), #10 (DHS), #11 (SSA) |
| Texas Driver's License | #3 (SMU), #12 (Texas DPS) |

## 🛠️ Tech Stack

- **Environment:** Google Colab
- **Document Processing:** PyPDF, BeautifulSoup4, LangChain
- **Embeddings:** sentence-transformers (all-MiniLM-L6-v2)
- **Vector Store:** FAISS
- **Evaluation:** RAGAS
- **UI:** Gradio
- **Hosting:** Hugging Face Spaces

## 📈 Chunking Statistics

- **Chunk size:** 600 characters
- **Chunk overlap:** 100 characters
- **Total documents:** 12
- **Total chunks:** [Run notebook to compute]
- **Average chunk length:** ~550 characters

## 🚀 How to Run

1. **Clone the repository:**
   ```bash
   git clone https://github.com/[your-username]/smu-international-student-assistant.git
   ```

2. **Open in Google Colab:**
   - Upload `SMU_International_Student_Assistant.ipynb` to Colab
   - Upload the 12 documents when prompted

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run all cells** to:
   - Load and clean documents
   - Chunk with metadata
   - Generate embeddings
   - Build vector store
   - Test retrieval

## 📝 Session Progress

- [x] **Session 1:** Domain selection, problem statement
- [x] **Session 2:** Data ingestion, text cleaning
- [x] **Session 3:** Chunking with metadata, statistics
- [ ] **Session 4:** Embeddings, vector store, retrieval
- [ ] **Session 5:** RAG chain, evaluation, deployment
- [ ] **Session 6:** Demo day

## 👤 Author

**Vee Huynh** - MSCS in Machine Learning Student at Southern Methodist University (SMU)

## 📄 License

This project is for educational purposes as part of the VietSpark Break Into Tech RAG Competition.
