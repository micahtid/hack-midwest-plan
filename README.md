# Startup Idea Validator

A platform that validates startup ideas by comparing them against existing backed startups. Using vector search and generative AI, the system finds competitors, identifies market gaps, and suggests possible pivots.

## Front-End

**Stack**: Next.js, React, TailwindCSS

### Components

**Submission Form**
- Allows users to enter their startup idea (title, description, features)
- Sends this data as a request to the back-end

**Retrieved Startups**
- Displays similar startups from the returned result
- Shows the data returned from the vector search

**Results & Data**
- Shows competitor analysis and pivot suggestions from the returned result
- Visualizes information through charts and graphs

**AI Chatbot**
- Enables users to ask questions about and edit information
- Uses Gemini API key
- Takes in the result data to provide conversational responses

**Actionable Buttons**
- Allows users to send new requests based on updated information
- Allows users to export results

### Structure
```
front-end/
│
├── components/
└── pages/
```

### Running
```bash
cd front-end
npm install
npm run dev
```
Runs the Next.js client at `http://localhost:3000`.

## Back-End

**Stack**: FastAPI, MongoDB, Atlas Vector Search, Selenium, ChatGPT, Gemini

### How It Works

**Populating MongoDB**
1. Selenium scrapes startup data from YCombinator directory
2. ChatGPT enriches the data by adding missing information (e.g. funding details)
3. Script pushes the processed data to MongoDB

**Handling Front-End Requests**
1. FastAPI receives requests and embeds the startup idea
2. MongoDB Atlas Vector Search queries the database and returns the most relevant startups
3. User input + retrieved data are passed to Gemini for competitor analysis, market gaps, and pivot suggestions

### Structure
```
back-end/
│
├── db/                     # Database setup & schema
│   ├── init_db.py
│   └── models.py
│
├── scraping/        
│   ├── yc_scraper.py       # Scrapes YCombinator directory
│   └── utils.py            # Pushes results to MongoDB
│
├── embeddings/             # Vector embedding function
│   └── embedder.py
│
└── main.py                 # FastAPI entry point
```

### Running
```bash
cd back-end
python -m venv venv
source venv/bin/activate   # On Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload
```
Runs the FastAPI server at `http://localhost:8000`.
