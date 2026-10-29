---
title: "Urdu Reading Annotation Platform"
short_title: "Urdu Reading Annotation Platform"
seo_title: "Urdu Reading Annotation Platform for Speech AI (ASR) Ground Truth"
description: "A full-stack FastAPI, PostgreSQL and React platform built at LUMS DARE-RC to create gold-standard Urdu reading annotations, the ground truth for testing whether speech recognition (ASR) can automate oral reading fluency scoring for children in Pakistan."
tagline: "A production annotation system that turns recordings of children reading Urdu aloud into gold-standard data for evaluating speech AI in Pakistani classrooms."
summary: "Full-stack platform (FastAPI, PostgreSQL, React) that produces ground-truth Urdu reading annotations, used to test whether ASR can replace manual oral-reading-fluency scoring."
order: 1
year: 2026
role: "Designer & full-stack developer"
domain: "AI data infrastructure · EdTech"
icon: "fa-microphone"
github: "https://github.com/Basit-Rahim/Annotation_plateform"
image: "/img/projects/urdu-reading-annotation-platform/hero.jpg"
image_alt: "Layer 2 word-by-word scoring workspace with an audio waveform and right-to-left Urdu Nastaliq words, each marked correct or error"
caption: "The Layer 2 workspace, running locally. An annotator scrubs the waveform, places the first-word, last-word and 60-second marks, and scores every printed Urdu word as correct or as a tagged error."
languages: [Python, TypeScript, SQL]
tags: [FastAPI, PostgreSQL, React, TypeScript, SQLAlchemy, Alembic, Docker, Caddy, Speech AI, ASR evaluation, Data annotation, Urdu NLP]
keywords: [Urdu annotation tool, ASR ground truth, oral reading fluency, EGRA, WCPM, speech recognition evaluation, FastAPI React PostgreSQL]
metrics:
  - { value: "2 layers", label: "verbatim transcript plus word-level scoring for each recording" }
  - { value: "16", label: "built-in graded Urdu passages (P01–P16)" }
  - { value: "67", label: "automated backend tests against real PostgreSQL" }
  - { value: "120 min", label: "rolling task lease, with autosaved drafts restored on reopen" }
gallery:
  - file: admin-dashboard.jpg
    alt: "Admin dashboard showing recordings, available tasks, in-progress tasks, submissions and annotators"
    title: "Admin dashboard: pipeline health at a glance"
    text: "Admins see how many recordings are ingested and how many tasks are available, in progress or submitted. They can also withdraw or delete a recording, for example after a mistaken ingest or a consent issue."
  - file: passages.jpg
    alt: "Passage management page listing graded Urdu passages with word counts"
    title: "Passage library"
    text: "Reading passages are stored as numbered word sequences, so every annotation lines up word by word with the printed text the child saw. A passage can't be deleted while any recording uses it, which keeps references intact."
---

## The problem

Measuring how well a child reads is one of the most important signals in education research. The standard test, an EGRA-style **oral reading fluency** assessment, counts **words correct per minute (WCPM)**: a trained assessor listens to a Grade 2–3 child read a passage for 60 seconds and marks every mistake by hand. That's slow and expensive, and it doesn't scale to the large, nationally representative samples that education policy in Pakistan needs.

**Automatic speech recognition (ASR)** could do this scoring automatically. Before anyone can trust it, though, they need **human ground truth**: reliable reference annotations to measure the model against. This platform produces that ground truth. I built it at **LUMS DARE-RC** in collaboration with the **LUMS AI Hub**.

## What I built

A secure web platform where trained annotators produce two independent annotations for every 60-second recording:

- **Layer 1, verbatim transcript.** A right-to-left **Noto Nastaliq** editor with one-click tags for `[unintelligible]`, `[non-target speech]` and `[code-switch]`, capturing exactly what the child said.
- **Layer 2, word-by-word scoring.** One row per printed word with a Correct/Error chip and a closed error taxonomy, plus *insert-word* slots for words the child added. The annotator places three draggable marks on the waveform: first word, last word and the 60-second boundary.

From these two layers you can compute WCPM and compare it directly with ASR output.

## Engineering highlights

- **Concurrency-safe task pool.** *Take next* hands each annotator the oldest available task through `SELECT … FOR UPDATE SKIP LOCKED` and a partial unique index, so two people can never claim the same recording. Annotators can't browse or cherry-pick tasks, which removes selection bias from the dataset.
- **Lease-based assignment.** Each task is leased for 120 minutes after the last edit. A background **APScheduler** job returns expired leases to the pool every minute, and an annotator whose lease lapsed gets their own unfinished task back first.
- **Autosave and recovery.** Drafts save to the browser on every change and to the server a moment later. A crash, closed tab or dropped connection restores the exact state when the task reopens.
- **Research-grade export.** Admins export every submitted annotation as JSON with Urdu text normalised to the study's annex rules, ready for WCPM computation and ASR benchmarking.
- **Production deployment.** Docker Compose with **Caddy** for automatic HTTPS. PostgreSQL and the API aren't exposed publicly, nightly database and audio backups run automatically, and a Google Cloud runbook documents deployment.
- **One-command local setup.** `uv run dev.py` provisions Python, an embedded PostgreSQL 16 and Node through uv wheels, then runs migrations and seeds the admin. Nothing is installed system-wide.

## Why it matters

Reliable ASR scoring would let researchers measure early-grade reading across thousands of children at a fraction of today's cost, giving policymakers faster evidence about which interventions actually improve learning. This platform is the measurement foundation that makes evaluating such a system possible.

> **Running it locally:** I cloned the repository and started the full stack with `uv run dev.py` (PostgreSQL, FastAPI and the Vite/React UI). I then ingested a synthetic test recording and captured these screenshots from a real annotator session. No real child audio is shown.
