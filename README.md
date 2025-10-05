# Startup Idea Validator

A platform that validates startup ideas by comparing them against existing backed startups. Using vector search and generative AI, the system finds competitors, identifies market gaps, and suggests possible pivots.

## How It Works

1. **User Submission (Front-End)**: User enters their startup idea in the web form.
2. **Back-End Processing**: FastAPI receives the request, embeds the idea, and queries MongoDB with Atlas Vector Search.
3. **Result Retrieval**: Relevant startups are returned from the database.
4. **AI Analysis**: User input + retrieved data are passed to Gemini for competitor analysis, gaps, and pivot suggestions.

## Tech Stack

- **Next.js + React + TailwindCSS (TypeScript)** → Front-end form & results display
- **FastAPI (Python)** → API layer between front-end, DB, and AI
- **MongoDB + Atlas Vector Search** → Stores startup data & embeddings, enables similarity search
- **Selenium (Python)** → Scrapes accelerator/VC-backed startup data
- **Embedding Model** → Converts text into vectors for storage & search
- **Gemini (Generative AI)** → Provides insights from retrieved data

## Set Up

### 1. MongoDB

- Create an Atlas cluster and a database (e.g. `startup_db`)
- Create a collection (e.g. `startups`) with fields: `name`, `description`, `funding`, `date`, `embedding`

### 2. Vector Search Index

In Atlas, create a search index for the `embedding` field:

```json
{
  "mappings": {
    "dynamic": false,
    "fields": {
      "embedding": {
        "type": "knnVector",
        "dimensions": 1536,
        "similarity": "cosine"
      }
    }
  }
}
```

### 3. Embeddings

- Use an embedding model (e.g. OpenAI, Hugging Face)
- Store the vector in `embedding` for each startup document

### 4. Data Scraping

- Run Selenium scripts (`yc_scraper.py`, `techstars_scraper.py`) to collect data
- Embed scraped descriptions
- Insert into MongoDB

### 5. Generative AI

- Get Gemini API key
- Store in `.env` as `GEMINI_API_KEY`
- Back-end uses Gemini for analysis

## Running the Project

### Front-End

```bash
cd front-end
npm install
npm run dev
```

Runs the Next.js client at `http://localhost:3000`.

### Back-End

```bash
cd back-end
python -m venv venv
source venv/bin/activate   # On Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload
```

Runs the FastAPI server at `http://localhost:8000`.

## Project Structure

```
startup-idea-validator/
│
├── front-end/              # Next.js + React client
│   ├── components/         # Reusable UI components
│   ├── pages/              # Routes (e.g. /submit, /results)
│   └── styles/             # TailwindCSS styles
│
├── back-end/               # Python FastAPI server
│   ├── api/                # Defines API routes (e.g. /analyze, /search)
│   ├── db/                 # Database connection & schema
│   ├── scraping/           # Selenium scrapers for startup data
│   ├── embeddings/         # Code to generate embeddings
│   ├── services/           # Core logic (vector search, AI calls)
│   └── main.py             # FastAPI entry point
│
├── docs/                   # Documentation
├── .env.example            # Env variable template
├── requirements.txt        # Python dependencies
├── package.json            # Front-end dependencies
└── README.md
```
