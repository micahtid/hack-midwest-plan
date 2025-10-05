# Startup Idea Validator
A platform that validates startup ideas by comparing them against existing backed startups. Using vector search and generative AI, the system finds competitors, identifies market gaps, and suggests possible pivots.

## How It Works
1. **User Submission**: User enters their startup idea in the web form
2. **Back-End Processing**: FastAPI receives the request, embeds the idea, and queries MongoDB with Atlas Vector Search
3. **Result Retrieval**: Relevant startups are returned from the database
4. **AI Analysis**: User input + retrieved data are passed to Gemini for competitor analysis, gaps, and pivot suggestions

## Tech Stack
- **Next.js + React** – Front-end for idea submission and results display  
- **FastAPI (Python)** – Handles API requests and backend logic  
- **MongoDB + Atlas Vector Search** – Stores startup data and embeddings for similarity search  
- **Selenium (Python)** – Scrapes startup data from accelerators/VCs  
- **Embedding Model** – Converts text into vector representations  
- **Gemini (Generative AI)** – Analyzes retrieved data and generates insights  

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
├── front-end/              
│   ├── components/         
│   └── pages/    
│
├── back-end/               
│   ├── db/                 # Database setup & schema
│   │   ├── init_db.py
│   │   └── models.py
│   │
│   ├── scraping/           
│   │   ├── yc_scraper.py
│   │   ├── techstars_scraper.py
│   │   └── utils.py
│   │
│   ├── embeddings/         
│   │   └── embedder.py
│   │
│   ├── services/           
│   │   ├── vector_search.py
│   │   ├── genai.py
│   │   └── pipeline.py
│   │
│   └── main.py             # FastAPI entry point
│            
├── .env.example            
├── requirements.txt        # Python dependencies             
└── README.md
```
