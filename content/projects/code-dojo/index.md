---
title: "CodeDojo"
date: 2026-03-13T10:30:00-04:00
externalLink: "https://code-dojo.tzvi.dev/"
featuredImage: "projects/code-dojo/hero.png"
description: "A collaborative coding interview practice platform with shared sessions, real-time editing, and in-browser code execution"
---

CodeDojo is a collaborative coding interview practice platform built for running live technical interview sessions in the browser. Interviewers can create a session, share a link, and work through problems with candidates in real time without any local setup.

The app combines a FastAPI backend, a React frontend, PostgreSQL session storage, and WebSockets for synchronized editing. Code execution runs in isolated E2B sandboxes, which keeps the experience fast for users while avoiding the risks of running arbitrary code on the app server. The production app is deployed as a single Docker image on Render, with Supabase providing the database.

Key features include:

- Shared coding sessions with live updates for all participants
- Multiple language options for interview practice
- Safe in-browser code execution
- Interview-style workflows built around link-based session sharing

[GitHub Link](https://github.com/Tadwork/code-dojo)
