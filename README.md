# Career RAG Chatbot

Sistem RAG (Retrieval-Augmented Generation) untuk analisis lowongan kerja menggunakan Groq LLM dan FAISS vector store.

## 📋 Deskripsi

Career RAG Chatbot adalah aplikasi yang menggunakan teknik RAG untuk menjawab pertanyaan tentang lowongan kerja berdasarkan data yang telah di-index. Sistem ini menggabungkan:

- **Retrieval**: Mencari dokumen relevan menggunakan FAISS vector store
- **Augmented Generation**: Menghasilkan jawaban menggunakan Groq LLM dengan context yang di-retrieve

## 🚀 Fitur

- ✅ Pencarian semantik dokumen lowongan kerja
- ✅ Jawaban berbasis context dari data yang tersedia
- ✅ Metadata tracking untuk setiap sumber jawaban
- ✅ Support untuk Google Colab dan local development
- ✅ Vector store yang dapat disimpan dan dimuat ulang

## 📦 Requirements

### Dependencies

```
groq
langchain
langchain-community
langchain-huggingface
faiss-cpu
tiktoken
python-dotenv
pandas
```

### API Keys

1. **Groq API Key**: Dapatkan dari [Groq Console](https://console.groq.com/)
2. **HuggingFace Token** (opsional): Untuk akses ke model private

## 🛠️ Instalasi

### 1. Clone atau download repository

```bash
git clone <repository-url>
cd rag_finpro
```

### 2. Install dependencies

```bash
pip install groq langchain langchain-community langchain-huggingface faiss-cpu tiktoken python-dotenv pandas
```

### 3. Setup API Key

#### Untuk Google Colab:
1. Buka Google Colab Secrets
2. Tambahkan `GROQ_API_KEY` dengan API key Anda

#### Untuk Local Development:
1. Buat file `.env` di root directory
2. Tambahkan:
   ```
   GROQ_API_KEY=your_groq_api_key_here
   ```

## 📊 Data Format

Sistem ini membutuhkan file CSV dengan kolom berikut:

- `Job Title`: Judul posisi
- `Company Name`: Nama perusahaan
- `Location`: Lokasi pekerjaan
- `Salary Estimate`: Estimasi gaji
- `Job Description`: Deskripsi pekerjaan
- `Industry`: Industri
- `Sector`: Sektor
- `Founded`: Tahun didirikan
- `Rating`: Rating perusahaan
- `python_yn`, `R_yn`, `excel`, `aws`, `spark`: Binary flags untuk skills

## 🔧 Penggunaan

### 1. Buka Notebook

Buka `career_rag_chatbot.ipynb` di Jupyter Notebook atau Google Colab.

### 2. Jalankan Sel Secara Berurutan

Notebook sudah diorganisir dalam beberapa section:

1. **Instalasi Dependencies**: Install semua package yang diperlukan
2. **Import Libraries**: Import semua library yang digunakan
3. **Konfigurasi API**: Setup Groq API key
4. **Data Loading & Cleaning**: Load dan clean data CSV
5. **Document Processing**: Build dokumen terstruktur
6. **Text Chunking**: Split dokumen menjadi chunks
7. **Vector Store Creation**: Buat dan simpan FAISS vector store
8. **RAG Functions**: Definisi fungsi-fungsi RAG
9. **Usage Examples**: Contoh penggunaan

### 3. Query System

```python
# Contoh penggunaan
query = "What skills are required for a Data Scientist?"
answer, sources = rag_answer(query)

print("ANSWER:\n", answer)
print("\nSOURCES:\n", sources)
```

## 📚 Fungsi Utama

### `clean_company_name(name)`
Membersihkan nama perusahaan dari rating yang tertempel di akhir.

**Parameters:**
- `name` (str): Nama perusahaan

**Returns:**
- `str`: Nama perusahaan yang sudah dibersihkan

### `build_document(row)`
Membangun dokumen terstruktur dari baris DataFrame.

**Parameters:**
- `row` (pd.Series): Baris DataFrame

**Returns:**
- `str`: Dokumen terstruktur

### `clean_text(text)`
Membersihkan teks dari multiple newlines dan spasi berlebih.

**Parameters:**
- `text` (str): Teks yang akan dibersihkan

**Returns:**
- `str`: Teks yang sudah dibersihkan

### `retrieve_docs(query, k=4)`
Melakukan retrieval dokumen yang relevan dengan query.

**Parameters:**
- `query` (str): Query dari user
- `k` (int): Jumlah dokumen yang akan di-retrieve (default: 4)

**Returns:**
- `tuple`: (context_string, metadata_list)

### `build_prompt(user_query, retrieved_context)`
Membangun prompt untuk LLM dengan context yang di-retrieve.

**Parameters:**
- `user_query` (str): Pertanyaan dari user
- `retrieved_context` (str): Context yang di-retrieve

**Returns:**
- `str`: Prompt yang sudah diformat

### `rag_answer(query, model="llama-3.1-8b-instant", k=4)`
Fungsi utama untuk menjawab query menggunakan RAG pipeline.

**Parameters:**
- `query` (str): Pertanyaan dari user
- `model` (str): Model LLM yang akan digunakan (default: "llama-3.1-8b-instant")
- `k` (int): Jumlah dokumen untuk retrieval (default: 4)

**Returns:**
- `tuple`: (answer, metadata_list)

## ⚙️ Konfigurasi

### Model Settings

- **LLM Model**: `llama-3.1-8b-instant` (default, dapat diubah)
- **Embedding Model**: `sentence-transformers/all-MiniLM-L6-v2`
- **Chunk Size**: 500 characters
- **Chunk Overlap**: 20 characters
- **Retrieval Count**: 4 dokumen (default)

### Vector Store

Vector store disimpan di folder `career_faiss_index/`. Untuk memuat ulang:

```python
vectorstore = FAISS.load_local(
    "career_faiss_index",
    embedding_model,
    allow_dangerous_deserialization=True
)
```

## 📝 Struktur Project

```
rag_finpro/
├── career_rag_chatbot.ipynb    # Main notebook
├── README.md                    # Dokumentasi
├── eda_data.csv                 # Data lowongan kerja (required)
├── career_faiss_index/          # Saved vector store (generated)
└── .env                         # Environment variables (optional)
```

## 🔍 Contoh Query

1. **Skills Query**: "What skills are required for a Data Scientist?"
2. **Job Description**: "What are the responsibilities of a Data Analyst?"
3. **Company Info**: "Tell me about companies hiring Data Scientists"
4. **Location Query**: "What jobs are available in New York?"

## ⚠️ Catatan Penting

1. **Data Privacy**: Pastikan data yang digunakan tidak mengandung informasi sensitif
2. **API Limits**: Perhatikan rate limits dari Groq API
3. **Vector Store**: Rebuild vector store jika data berubah
4. **Model Selection**: Pilih model yang sesuai dengan kebutuhan dan budget

## 🐛 Troubleshooting

### Error: "GROQ_API_KEY not found"
- Pastikan API key sudah di-set di environment atau `.env` file
- Untuk Colab, pastikan secret sudah ditambahkan

### Error: "HuggingFaceEmbeddings deprecated"
- Sudah diperbaiki dengan menggunakan `langchain-huggingface`
- Pastikan package `langchain-huggingface` sudah terinstall

### Vector Store tidak memiliki metadata
- Pastikan menggunakan `FAISS.from_documents()` bukan `FAISS.from_texts()`
- Rebuild vector store dengan format yang benar

## 📄 License

Project ini bebas digunakan untuk keperluan pembelajaran dan pengembangan.

## 🤝 Kontribusi

Kontribusi sangat diterima! Silakan buat issue atau pull request.

## 📧 Kontak

Untuk pertanyaan atau saran, silakan buat issue di repository ini.

---

**Dibuat dengan ❤️ menggunakan LangChain, Groq, dan FAISS**


