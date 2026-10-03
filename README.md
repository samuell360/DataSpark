# DataSpark

I built DataSpark because I wanted a better way for people to learn statistics—one focused on interactive simulations and real-time feedback instead of just reading textbooks. It's a full-stack platform designed to help users experiment with statistical concepts, see how data changes in real-time, and get contextual help along the way.

![DataSpark Dashboard](screenshots/01-hero-dashboard.png)

## What is this?
DataSpark is an interactive learning environment I engineered to make statistics more intuitive. Users can run statistical simulations, explore datasets, track their learning progress, and ask a local AI assistant questions when they get stuck. 

I designed the architecture to be fast and self-contained. The backend is built with FastAPI and PostgreSQL, and the frontend is a React SPA using Vite. I also integrated a local RAG (Retrieval-Augmented Generation) pipeline using Ollama so the learning assistant can provide answers based strictly on verified statistical definitions without relying on external APIs.

## Why I Built It
Learning statistics can be dry and intimidating. I wanted to build a project that solved a real problem while giving me hands-on experience building a complex, full-stack application from scratch. This project pushed me to learn about:
- Managing a React frontend with complex state.
- Building a performant Python backend with FastAPI and SQLAlchemy.
- Setting up a local, privacy-first AI pipeline with FAISS and Ollama.
- Deploying a full system natively on a Linux server using systemd and Nginx.

## Key Features

* **Interactive Simulations**: Visualizations that let users manipulate variables and instantly see how statistical distributions and outcomes change.
* **Progress Tracking**: A gamified learning path with modules, lessons, and a dashboard to track progress.
* **Local RAG Assistant**: A completely local AI assistant that helps answer questions based on a curated set of statistical concepts.
* **Authentication & Profiles**: Secure user registration and login using JWTs and bcrypt password hashing.

## Tech Stack

Here’s what I used to build the platform:

**Frontend**
* React 18 (Vite)
* TypeScript
* Tailwind CSS
* React Router & Context API for state management

**Backend**
* FastAPI (Python)
* PostgreSQL (production) & SQLite (local dev)
* SQLAlchemy (ORM) & Alembic (Migrations)
* Custom JWT Authentication & bcrypt

**AI & Search (Local)**
* Ollama (Llama 3 for generation)
* FAISS (Dense vector retrieval)
* BM25 (Lexical search)
* Cross-Encoder (Reranking)

**Deployment**
* Native Ubuntu Linux
* systemd (Process management)
* Nginx (Reverse proxy)

## Project Status

This repository is a public showcase of the project's architecture and design. The actual application source code is private. If you're a recruiter or hiring manager and would like a live demo or a deeper dive into the code, I'd be happy to walk you through it!

---
*For more details on how the system is built, check out the [Architecture Overview](docs/architecture.md).*
