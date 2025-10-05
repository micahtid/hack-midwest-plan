# Backend Components

## 1. Data Scraping
- Use Selenium to navigate accelerator websites (YC, Techstars)
- Extract startup data: names, descriptions, funding information
- Store raw data for processing

## 2. Embeddings Generation
- Convert startup descriptions into vector representations
- Use Cohere's embed-english-light-v3.0 model (free tier, fast)
- Generate 384-dimensional vectors for each startup

## 3. Database Storage
- Store startup data in MongoDB Atlas
- Include vector embeddings in each document
- Set up vector search index for similarity queries

## 4. Vector Search
- Query database with user's startup idea
- Find similar startups using cosine similarity
- Return top N most relevant competitors

## 5. Generative AI Analysis
- Pass user input and retrieved startups to Gemini
- Generate competitive analysis
- Identify market gaps and suggest pivots
