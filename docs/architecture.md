# Architecture Overview

When I started designing DataSpark, my main goal was to build a system that was robust, easy to maintain, and capable of running a local AI pipeline without depending on paid cloud APIs. I decided on a decoupled architecture with a React frontend, a FastAPI backend, and a PostgreSQL database, all running natively on a Linux server.

![DataSpark Architecture](../diagrams/dataspark-architecture.png)

## High-Level Design

The system is split into three main pieces:
1. **Frontend (React/Vite)**: Handles all the interactive simulations, user dashboards, and real-time feedback.
2. **Backend API (FastAPI)**: Manages business logic, user authentication, learning progress tracking, and database interactions.
3. **Local AI Pipeline (Ollama + FAISS)**: A dedicated retrieval-augmented generation (RAG) system running entirely locally to provide context-aware help.

## The Frontend

I built the frontend as a Single Page Application (SPA) using React 18 and Vite. 
- **Language**: TypeScript, because catching type errors early saved me hours of debugging.
- **Styling**: Tailwind CSS for rapid, consistent UI development.
- **Routing & State**: I used React Router with `Suspense` and lazy loading to keep the initial bundle size small. Authentication state is managed globally using React Context.

The hardest part here was managing the state for the interactive simulations. I had to make sure the UI felt responsive when users adjusted sliders and variables.

## The Backend

I chose FastAPI for the backend because I wanted to write asynchronous Python code and take advantage of automatic OpenAPI documentation.

- **Database**: PostgreSQL. I used `asyncpg` to handle database connections asynchronously, which keeps the API snappy.
- **ORM**: SQLAlchemy for defining models, with Alembic handling schema migrations.
- **Authentication**: Custom JWT-based authentication. Passwords are securely hashed using `bcrypt` before hitting the database. I deliberately avoided third-party auth providers like OAuth to keep the system entirely self-contained.

### Testing
I wrote comprehensive end-to-end (E2E) and contract tests using `pytest` to make sure the critical flows—like user registration, logging in, and completing a lesson—never break.

## The Local RAG Pipeline

One of the coolest parts of this project is the AI assistant. I didn't want to rely on OpenAI or other external APIs because of privacy concerns and latency. Instead, I built a completely local Retrieval-Augmented Generation (RAG) pipeline.

Here’s how it works when a user asks a question:
1. **Retrieval**: The system searches a curated database of statistical concepts using two methods: FAISS (for semantic meaning) and BM25 (for exact keyword matches).
2. **Reranking**: A local cross-encoder model reranks the combined results to find the most relevant context.
3. **Generation**: The context and the user's question are sent to a local Ollama instance, which streams the answer back to the frontend.

## Deployment

Initially, I looked into Docker Compose, but I decided to deploy everything natively on an Ubuntu Linux server. I wanted the experience of configuring a production Linux environment from scratch.

- **Process Management**: I wrote custom `systemd` service files to manage the FastAPI application and the Ollama runtime. This ensures they start automatically on boot and restart if they crash.
- **Reverse Proxy**: Nginx sits in front of everything, serving the compiled React static files and routing API requests to the FastAPI backend.
