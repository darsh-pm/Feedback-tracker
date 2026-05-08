# Pulse — Feedback Tracker

A PM feedback collection and analysis tool built with React, 
Supabase, and AI.

## Live Demo
https://darshan2study-cpu.github.io/Feedback-tracker/

## Features
- Log feedback by source, category, and sentiment
- Track feedback through Open → In Progress → Addressed
- Category breakdown with visual progress bars
- Multi-filter system (sentiment, status, source)
- AI analysis via Groq — identifies top pain points, what users 
  love, and what to prioritize fixing first
- Email authentication with per-user data isolation
- Click any feedback card to view full details and edit description

## Stack
- React (via CDN, no build step)
- Supabase (PostgreSQL database + Auth + Row Level Security)
- Groq API (Llama 3.3-70b)
- Tailwind CSS
- GitHub Pages

## Architecture
Frontend React app → Supabase database (per-user RLS) → Groq AI API

## Planned Enhancements
- Activity log per feedback item
- Supabase Edge Function to secure Groq API key
- Google Forms integration to auto-import feedback
