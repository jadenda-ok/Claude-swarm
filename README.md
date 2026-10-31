# 🐝 claude-swarm

> A parallel agent swarm for codebase analysis, built on the [Claude Agent SDK](https://docs.anthropic.com/en/api/agent-sdk/overview).

`claude-swarm` fans out independent specialist agents against a codebase, runs them concurrently, and prints a unified report. It's a minimal, readable example of the **fan-out / fan-in** swarm pattern — under 200 lines of Python, no framework magic.

[![CI](https://github.com/yourname/claude-swarm/actions/workflows/ci.yml/badge.svg)](https://github.com/yourname/claude-swarm/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)

---

## ✨ What it does

Three specialist agents run concurrently, each with its own context window and tool allowlist:

| Agent | Purpose |
| :--- | :--- |
| **security-auditor** | Injection, broken auth, insecure crypto, exposed secrets |
| **performance-analyst** | N+1 queries, blocking I/O, memory risks |
| **test-coverage-critic** | Untested paths, brittle assertions |

Their reports are then optionally synthesized by a **lead agent** that deduplicates findings and ranks them by severity.

---

## 🏗️ Architecture

Each worker is a **fully independent Claude session**. No shared context, no cross-talk. They see only the codebase and their system prompt.

---

## 🚀 Quick start

```bash
git clone https://github.com/yourname/claude-swarm.git
cd claude-swarm
python3 -m venv .venv && source .venv/bin/activate
pip install -e .

export ANTHROPIC_API_KEY="sk-ant-..."
claude-swarm /path/to/your/project
