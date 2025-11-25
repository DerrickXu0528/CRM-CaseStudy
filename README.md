# AI-Native CRM Lead Management System

A full-stack web application for managing consulting leads with AI-powered quality scoring using Claude API.

## 🎯 Features

- **CSV Upload**: Import leads from CSV files
- **Lead Management**: View, filter, and manage consulting leads
- **AI Scoring**: Automatically score lead quality using Claude AI (0-100 scale)
- **Smart Filtering**: Filter by industry, location, and AI score
- **Batch Processing**: Score multiple leads at once
- **RESTful API**: Full backend API with interactive documentation

## 🏗️ Architecture

### Tech Stack

**Backend:**
- Python 3.11+
- FastAPI (REST API framework)
- SQLAlchemy (ORM)
- SQLite (Database)
- Anthropic Claude API (AI scoring)
- Pandas (CSV processing)

**Frontend:**
- HTML5
- Tailwind CSS (styling)
- Vanilla JavaScript (no frameworks)

**Deployment:**
- Backend: Render
- Frontend: Vercel
- Database: SQLite (file-based)

### Architecture Diagram
```
┌─────────────┐         ┌──────────────┐         ┌─────────────┐
│   Frontend  │────────>│   Backend    │────────>│   Database  │
│   (Vercel)  │  HTTP   │   (Render)   │  SQL    │   (SQLite)  │
└─────────────┘         └──────────────┘         └─────────────┘
								│
								│ API Call
								▼
						┌──────────────┐
						│  Claude API  │
						│  (Anthropic) │
						└──────────────┘
```

## 🚀 Setup Instructions

### Prerequisites

- Python 3.11 or higher
- Git
- Anthropic API key ([Generate Claude API Key Here](https://console.anthropic.com/))

### Local Development Setup

#### Backend Setup
```bash
# Clone the repository
git clone https://github.com/DerrickXu0528/CRM-CaseStudy-Backend.git
cd CRM-CaseStudy-Backend

# Create virtual environment
python -m venv venv

# Activate virtual environment
# Windows:
venv\Scripts\activate
# Mac/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Create .env file
echo "ANTHROPIC_API_KEY=..." > .env

# Run the server
uvicorn main:app --reload
```

Backend will run at: `http://127.0.0.1:8000`

API Docs available at: `http://127.0.0.1:8000/docs`

#### Frontend Setup
```bash
# Clone the repository
git clone https://github.com/DerrickXu0528/CRM-CaseStudy-Frontend.git
cd CRM-CaseStudy-Frontend

# Use Python HTTP server
python -m http.server 3000
```

Frontend will run at: `http://localhost:3000`

## 📖 API Endpoints

### Health Check
```
GET / - Check if API is running
```

### Lead Management
```
POST /upload - Upload CSV file of leads
GET /leads - Get all leads (with optional filters)
GET /leads/{id} - Get single lead details
DELETE /leads/{id} - Delete a lead
```

### AI Scoring
```
POST /leads/{id}/score - Score a lead with Claude AI
```

### Filters
```
GET /filters - Get unique industries and locations
```

## 🎨 Design Decisions & Trade-offs

### 1. SQLite vs PostgreSQL
**Decision:** SQLite  
**Reason:**
- Zero configuration for development
- Perfect for MVP with <10k records
- Easy to backup (single file)
- Can migrate to PostgreSQL later if needed

**Trade-off:** Limited concurrent write operations (fine for single-user/small team use)

### 2. Vanilla JS vs React
**Decision:** Vanilla JavaScript  
**Reason:**
- No build process = faster development
- Smaller bundle size
- Easier to understand for evaluation
- Perfect for this scope

**Trade-off:** More manual DOM manipulation, no state management library

### 3. Single HTML File vs Component Architecture
**Decision:** Single file  
**Reason:**
- 8-hour time constraint
- Easier deployment
- Simple to review

**Trade-off:** Less maintainable for large-scale apps

### 4. File-based CSV vs Drag-and-Drop
**Decision:** File input  
**Reason:**
- Native browser feature (no libraries)
- Works on all devices
- Simpler implementation

**Trade-off:** Slightly less elegant UX

### 5. Synchronous AI Scoring vs Background Jobs
**Decision:** Synchronous (user waits for result)  
**Reason:**
- Simpler implementation
- Immediate feedback
- No need for job queue infrastructure

**Trade-off:** User must wait 2-3 seconds per lead

## 🧠 AI Scoring Logic

The AI agent analyzes leads based on:

1. **Industry Fit**: Is this a consulting client?
2. **Contact Completeness**: Quality of contact information
3. **Company Legitimacy**: Website, location indicators
4. **Notes Analysis**: Red flags or positive signals

Returns:
- **Score**: 0-100 (quality rating)
- **Justification**: 2-3 sentence explanation
- **Next Action**: Specific recommended step

## 🎬 Demo

**Live Demo:**
- Frontend: https://crm-case-study-frontend.vercel.app/
- API Docs: https://crm-casestudy.onrender.com/docs

**Demo Video:** https://1drv.ms/v/c/fc5de19042686150/IQCPib9SKMlSQJu0k-IlIm7kAeetvfE7jmFQju7_Qu0hbS4?e=TcLbdT

**GitHub:** 
- https://github.com/DerrickXu0528/CRM-CaseStudy-Backend
- https://github.com/DerrickXu0528/CRM-CaseStudy-Frontend

## 📊 Cost Breakdown

- **Render (Backend)**: Free tier ($0/month)
- **Vercel (Frontend)**: Free tier ($0/month)
- **Claude API**: ~$0.02 per lead score
- **Total for demo**: ~$3-4 (scoring 50-100 leads)

## 🔒 Security Considerations

- API key stored in environment variables (not in code)
- CORS restricted to specific domains in production
- No user authentication (out of scope for MVP)
- SQL injection protected by SQLAlchemy ORM

## 🚧 Known Limitations

1. **No authentication**: Anyone with URL can access
2. **No edit functionality**: Can only view/delete, not edit leads, need to edit in the csv file and upload again
3. **Single-threaded scoring**: Batch scoring is sequential
4. **No pagination**: All leads loaded at once (fine for <1000 leads)
5. **Basic error handling**: Could be more robust

## 🔮 Future Enhancements

If I had more time, I would add:

1. **User authentication** (OAuth or JWT)
2. **Edit lead functionality**
3. **Async job queue** for AI scoring (Celery + Redis)
4. **Pagination** for large datasets
5. **Export to CSV** with filters
6. **Email integration** to act on "next actions"
7. **PostgreSQL migration** for production scale
8. **Caching layer** (Redis) for frequently accessed data
9. **Automated testing** (pytest for backend, Jest for frontend)
10. **Real-time updates** (WebSockets)

## 📝 Assignment Assumptions

Per the instructions, I made these assumptions:

1. **CSV format**: Assumed standard business lead format
2. **AI scoring criteria**: Focused on consulting industry fit
3. **Deployment target**: Free tiers acceptable for demo
4. **Scale**: Optimized for 50-500 leads (startup/SME scale)
5. **Security**: Basic production security, not enterprise-grade
