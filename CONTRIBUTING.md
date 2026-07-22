# Contributing to Agentic Document Analyser

Thanks for your interest in contributing. This project (DocIntel Pro) is maintained by [AI Exponent LLC](https://aiexponent.com) and is in **Alpha** — interfaces and service boundaries may still change.

## Reporting bugs

Open a GitHub issue with:

- Which service is affected (orchestrator, preprocessing, visual, or frontend)
- Python version (`python --version`) and Node version (`node --version`) if relevant
- OS and whether you are running via Docker Compose or per-service `uvicorn`
- Steps to reproduce, plus expected vs actual behaviour
- A sample document is helpful when the issue is parsing-related (redact anything sensitive first)

## Development setup

The system is a set of Python 3.10+ services plus a Next.js frontend. Use a project-scoped Conda (or venv) environment; do not install into system Python.

```bash
conda create -n doc_analysis_env python=3.10 -y
conda activate doc_analysis_env

pip install -r preprocessing_service/requirements.txt
pip install -r visual_service/requirements.txt
pip install -r orchestrator/requirements.txt
```

PDF processing needs PopplerUtils (`brew install poppler` on macOS, `sudo apt-get install poppler-utils` on Ubuntu). The visual service needs a Fireworks AI API key:

```bash
export FIREWORKS_API_KEY="your_api_key_here"
```

Run the full stack with Docker Compose:

```bash
docker compose up
```

Or run each service in its own terminal:

```bash
uvicorn preprocessing_service.main:app --port 8001 --reload
uvicorn visual_service.main:app --port 8002 --reload
uvicorn orchestrator.main:app --port 8000 --reload
cd frontend && npm install && npm run dev   # UI on http://localhost:3000
```

See [ARCHITECTURE.md](ARCHITECTURE.md) for how the services fit together.

## Code contributions

1. Fork the repository and branch from `main`.
2. Keep a change scoped to one service where possible; note in the PR when a change crosses the orchestrator/worker boundary.
3. Add or update tests for the service you touch. The Python services use `pytest`:

   ```bash
   pytest visual_service/ preprocessing_service/ -v
   ```

4. Open a PR against `main` describing the change and how you verified it. CI (`.github/workflows/ci.yml`) must pass.

## License

By contributing, you agree that your contributions are licensed under the terms of this repository's [LICENSE](LICENSE).
