---
title: "Smart Code Reviewer: an LLM-Powered GitHub Action"
short_title: "Smart Code Reviewer"
seo_title: "Smart Code Reviewer: AI Pull Request Review GitHub Action with Llama 3.3 70B"
description: "An open-source GitHub Action that reviews every pull request with an LLM (Llama 3.3 70B on Groq) for readability, structure and maintainability, posting line-referenced findings with concrete fixes as a PR comment, with structured JSON output and graceful failure handling."
tagline: "A senior-engineer-style code review on every pull request, before a human looks at it. It's not a linter: it reads the diff for readability, structure and maintainability, and explains why each finding matters."
summary: "GitHub Action that sends each PR diff to Llama 3.3 70B (Groq) and posts a structured, line-referenced review of readability, structure and maintainability."
order: 4
year: 2026
role: "Author & maintainer"
domain: "LLM engineering · Developer tools"
icon: "fa-code"
github: "https://github.com/Basit-Rahim/AI-reviewer"
image: "/img/projects/smart-code-reviewer/hero.jpg"
image_alt: "The sample code under review on the left with flagged lines highlighted, and the AI review comment on the right grouped into Readability and Structure findings with code suggestions"
caption: "The repository's sample review of examples/bad_code.py, rendered next to the code. Each finding cites line numbers, explains why it matters and proposes a concrete fix, down to a suggested dataclass."
languages: [Python, Docker, YAML]
tags: [LLMs, Prompt engineering, Llama 3.3 70B, Groq, GitHub Actions, Docker, PyGithub, OpenAI SDK, Structured output, Developer tools, Code quality]
keywords: [AI code review GitHub Action, LLM pull request review, Llama 3.3 code review, Groq API, automated code review bot, prompt engineering]
metrics:
  - { value: "3", label: "review dimensions: readability, structure, maintainability" }
  - { value: "1 file", label: "of YAML to install: no servers, no config" }
  - { value: "100K", label: "character diff budget, truncated safely beyond that" }
  - { value: "Fail-safe", label: "API or parsing errors post an explanatory comment instead of failing CI" }
---

## The problem

Human code review is the most expensive quality gate in software teams. Much of a reviewer's time goes on issues that are easy to spot but tedious to explain: unclear names, god classes, copy-pasted logic, magic numbers, swallowed exceptions. Linters catch syntax and style, but they can't tell you that a class "handles validation, database queries *and* email notifications".

**Smart Code Reviewer** handles that layer automatically, so human reviewers can focus on design and correctness.

## How it works

```
developer opens or updates a PR
  → GitHub Action triggers (Docker container)
  → reviewer.py fetches changed files + diff via the GitHub API
  → diff + review-focused system prompt sent to Llama 3.3 70B on Groq
  → model returns findings as structured JSON
  → reviewer.py formats them into markdown and posts a PR comment
```

Installing it is a single workflow file and one repository secret:

{% raw %}
```yaml
- uses: Basit-Rahim/AI-reviewer@main
  with:
    groq-api-key: ${{ secrets.GROQ_API_KEY }}
```
{% endraw %}

## Engineering decisions

- **Structured output, not free text.** The system prompt asks the model for JSON findings, each with a category, line reference, issue and suggestion. The action parses the response and tolerates stray markdown fences, validates the schema, drops unknown categories with a warning, and renders a consistent comment every time.
- **Prompted like a senior engineer.** The 74-line system prompt defines three dimensions with concrete checklists: naming, nesting depth, single responsibility, DRY, type hints, input validation, hard-coded secrets and more. It also requires the model to say what the PR did *well*, so reviews stay constructive.
- **Fails safe.** API errors, rate limits and malformed model output never crash the workflow. The action posts a short comment explaining that the review couldn't be completed and asking for a manual review.
- **Bounded cost.** Diffs are built from per-file patches, binary files are skipped, and anything past 100,000 characters is truncated with an explicit marker.
- **Provider-portable.** It calls Groq through the OpenAI-compatible SDK, so switching to another hosted or self-hosted model is a configuration change. The model is already an input (`groq-model`).

## Example output

Run against [`examples/bad_code.py`](https://github.com/Basit-Rahim/AI-reviewer/blob/main/examples/bad_code.py), a realistic order service with deliberate problems, the reviewer found **4 readability issues, 3 structural concerns, 7 maintainability suggestions and 2 good practices**. These included a hard-coded production database password and live API key, promo logic copy-pasted three times, and bare `except:` blocks silently swallowing failures. A refactored [`good_code.py`](https://github.com/Basit-Rahim/AI-reviewer/blob/main/examples/good_code.py) shows each fix applied.

## Where it goes next

The README sets out a production roadmap: a self-hosted Llama or Mistral model so code never leaves the organisation, **RAG over the full codebase** for architectural context beyond the diff, a thumbs-up/down feedback loop that learns each team's standards, and per-repository rule profiles that complement existing linters.

> **Running it locally:** I installed the action's dependencies in a fresh virtual environment and exercised the parsing, formatting and failure paths of `reviewer.py`. The screenshot renders the repository's own sample review output. A live Groq call needs an API key, which isn't included here.
