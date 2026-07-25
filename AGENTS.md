# Repository Guidelines

## Project Structure & Module Organization

This repository contains Jira Story Planner Agent, a Jupyter-based LangGraph pipeline for fetching Jira stories, inspecting GitHub context through MCP tools, planning subtasks, and creating Jira subtasks under the source story. Python dependencies are listed in `requirements.txt`. Local configuration and secrets live in `.env`; keep example-safe configuration in `.env.example` if one is added.

The pipeline uses Moonshot AI Kimi K2.5 through AWS Bedrock as its LLM backend.

Generated and local-only folders such as `jira_agent_venv/` and `__pycache__/` are not source code and should not be committed. When the project grows, move reusable notebook logic into importable modules at the repository root, for example `jira_pipeline.py`, and place tests under `tests/`.

## Build, Test, and Development Commands

Create and activate a virtual environment:

```bash
python -m venv jira_agent_venv
jira_agent_venv\Scripts\activate
```

Install dependencies:

```bash
python -m pip install -r requirements.txt
```

Run the notebook:

```bash
jupyter notebook jira_agent.ipynb
```

If a Streamlit app module is present, run it with:

```bash
streamlit run streamlit_app.py
```

Run tests once added:

```bash
python -m pytest
```

## Coding Style & Naming Conventions

Use Python 3.11+ syntax, 4-space indentation, and type hints for public functions. Use `snake_case` for functions, variables, graph state keys, and node functions, such as `fetch_jira`, `fetch_github`, and `plan_subtasks`. Keep prompt text close to the node or agent that uses it, and keep MCP tool allowlists explicit.

## Testing Guidelines

No automated tests are currently present. Add `pytest` tests under `tests/` using `test_*.py` files and `test_*` functions. Mock external services: Jira MCP, GitHub MCP, AWS Bedrock, and network calls should not run in unit tests. Prioritize tests for environment validation, tool allowlist filtering, prompt construction, and graph state transitions.

## Commit & Pull Request Guidelines

No usable project commit history is available, so use concise imperative commit messages, for example `Add Streamlit pipeline UI` or `Tighten GitHub agent prompt`. Pull requests should include a short summary, verification commands, related Jira ticket IDs, and notes for any new environment variables, MCP scopes, or AWS Bedrock permissions.

## Security & Configuration Tips

Never commit `.env`, credentials, API tokens, virtual environments, notebook outputs containing secrets, or generated caches. Configure Jira, GitHub, and Bedrock through environment variables. Use the minimum MCP token scopes required for the tools the pipeline actually allows.
