# AI-Powered Smart Search & Recommendation System

A full-stack application featuring an intelligent search and recommendation engine. The frontend is built with React/TypeScript, and the backend uses a hybrid Express.js + Flask architecture powered by BERT-based ML models.

---

## Architecture Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  React Frontend  │    │  Express Server │    │  Flask Server   │
│                 │◄──►│                 │◄──►│                 │
│  - Vite + TS    │    │  - Authentication│    │  - BERT Models  │
│  - shadcn-ui    │    │  - Web Scraping │    │  - Recommendations│
│  - Tailwind CSS │    │  - NLP Processing│    │  - ML Pipeline  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │
                                ▼
                       ┌─────────────────┐
                       │   Supabase DB   │
                       │                 │
                       │  - User Data    │
                       │  - Search History│
                       │  - Item Data    │
                       │  - Embeddings   │
                       └─────────────────┘
```

---

## Tech Stack

### Frontend
- **Vite** — Fast build tooling and dev server
- **TypeScript** — Typed JavaScript
- **React** — UI library
- **shadcn-ui** — Component library
- **Tailwind CSS** — Utility-first CSS framework

### Backend
- **Express.js** (Port 3000) — API server, authentication, web scraping, NLP
- **Flask** (Port 5000) — BERT-based ML recommendations
- **Supabase** — PostgreSQL database with pgvector for vector similarity search

### Key Libraries
- **BERT / Sentence Transformers** — Natural language processing
- **Puppeteer & Cheerio** — Web scraping and HTML parsing
- **Natural** — NLP utilities
- **pgvector** — Vector similarity search

---

## Project Structure

```
project-root/
├── frontend/                   # React/Vite frontend
│   ├── src/
│   ├── package.json
│   └── ...
├── backend/
│   ├── express-server/
│   │   ├── src/
│   │   │   ├── config/
│   │   │   │   └── supabase.js
│   │   │   ├── controllers/
│   │   │   │   ├── authController.js
│   │   │   │   ├── searchController.js
│   │   │   │   └── recommendationController.js
│   │   │   ├── routes/
│   │   │   │   ├── auth.js
│   │   │   │   ├── search.js
│   │   │   │   └── recommendations.js
│   │   │   └── server.js
│   │   ├── package.json
│   │   └── .env
│   ├── flask-server/
│   │   ├── models/
│   │   │   └── recommendation_model.py
│   │   ├── routes/
│   │   │   ├── health.py
│   │   │   └── recommendations.py
│   │   ├── app.py
│   │   ├── config.py
│   │   ├── requirements.txt
│   │   └── .env
│   └── database/
│       └── schema.sql
└── README.md
```

---

## Prerequisites

- Node.js v16 or higher & npm
- Python v3.8 or higher
- A Supabase account and project

---

## Quick Start

### 1. Clone the Repository

```bash
git clone <YOUR_GIT_URL>
cd <YOUR_PROJECT_NAME>
```

### 2. Frontend Setup

```bash
# Install dependencies
npm i

# Start the development server
npm run dev
```

You can also edit files directly in GitHub or use GitHub Codespaces:
- Navigate to the file → click the pencil icon → commit changes.
- Or open the repo in a new Codespace via the green **Code** button → **Codespaces** tab.

### 3. Backend Setup

```bash
# Install Express server dependencies
cd backend/express-server
npm install

# Install Flask server dependencies
cd ../flask-server
pip install -r requirements.txt
```

### 4. Environment Configuration

#### Express Server (`backend/express-server/.env`)
```bash
cp env.example .env
```
```env
SUPABASE_URL=your_supabase_url_here
SUPABASE_ANON_KEY=your_supabase_anon_key_here
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key_here
PORT=3000
NODE_ENV=development
FLASK_SERVER_URL=http://localhost:5000
```

#### Flask Server (`backend/flask-server/.env`)
```bash
cp env.example .env
```
```env
FLASK_ENV=development
PORT=5000
SECRET_KEY=your-secret-key-here
SENTENCE_TRANSFORMER_MODEL=all-MiniLM-L6-v2
```

### 5. Database Setup

1. Create a new Supabase project.
2. Enable the **pgvector** extension in your Supabase project settings.
3. Run `backend/database/schema.sql` in the Supabase SQL editor.

### 6. Start All Servers

```bash
# Terminal 1 — Express server
cd backend/express-server
npm run dev

# Terminal 2 — Flask server
cd backend/flask-server
python app.py

# Terminal 3 — Frontend
npm run dev
```

---

## API Endpoints

### Authentication

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/signup` | Register a new user |
| POST | `/api/auth/signin` | Login an existing user |
| GET | `/api/auth/me` | Get current user info |

#### Signup Example

**Request:**
```json
{
  "firstname": "John",
  "lastname": "Doe",
  "email": "john.doe@example.com",
  "password": "securepassword123"
}
```
**Response:**
```json
{
  "message": "User created successfully",
  "user": {
    "id": "uuid",
    "email": "john.doe@example.com",
    "full_name": "John Doe"
  }
}
```

### Search

#### POST `/api/search`

**Request:**
```json
{
  "query": "best wireless headphones under $100",
  "userId": "user-uuid"
}
```
**Response:**
```json
{
  "success": true,
  "originalQuery": "best wireless headphones under $100",
  "processedQuery": {
    "keyTerms": ["wireless", "headphone", "best"],
    "categories": ["ecommerce"]
  },
  "results": [
    {
      "title": "Sony WH-CH720N Wireless Headphones",
      "price": "$99.99",
      "link": "https://amazon.com/...",
      "source": "Amazon",
      "type": "product"
    }
  ],
  "totalResults": 10
}
```

### Recommendations

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/recommendations` | Get personalized recommendations |
| GET | `/api/recommendations/trending` | Get trending recommendations |

#### POST `/api/recommendations`

**Request:**
```json
{
  "userId": "user-uuid",
  "query": "gaming accessories",
  "limit": 10
}
```
**Response:**
```json
{
  "success": true,
  "recommendations": [
    {
      "id": "item-uuid",
      "title": "Mechanical Gaming Keyboard",
      "category": "electronics",
      "price": 149.99,
      "similarity_score": 0.85,
      "recommendation_type": "bert_similarity"
    }
  ]
}
```

**GET `/api/recommendations/trending` query parameters:**
- `category` (optional) — Filter by category
- `limit` (optional) — Number of results (default: 10)

---

## Development Notes

### Adding New Scraping Sources

1. Create a new scraping function in `searchController.js`.
2. Add it to the appropriate category in `performWebScraping()`.
3. Update the NLP categorization in `categorizeQuery()`.

### Extending the Recommendation System

1. Modify the `RecommendationModel` class in `models/recommendation_model.py`.
2. Add new recommendation strategies.
3. Update the routes in `routes/recommendations.py`.

---

## Deployment

### Production Checklist

- Set all required environment variables (Supabase credentials, Flask secret key, etc.)
- Use **PM2** as a process manager for the Node.js/Express server
- Use **Gunicorn** for the Flask server
- Enable **HTTPS**
- Configure rate limiting
- Set up logging, monitoring, and database backups

---

## Contributing

1. Fork the repository.
2. Create a feature branch.
3. Make your changes and add tests where applicable.
4. Submit a pull request.

---

## License

This project is licensed under the MIT License.

## Support

For support and questions, please open an issue in the repository.
