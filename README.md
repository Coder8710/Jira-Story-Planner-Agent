# Jira Story Planner Agent

Jira Story Planner Agent is a LangGraph-based multi-agent system that turns Jira stories into codebase-grounded implementation subtasks and creates them under the source Jira story. It combines Jira story retrieval, GitHub repository analysis, planning, and Jira subtask creation into one coordinated workflow.

## Project Purpose

The project helps convert product work into engineering-ready tasks. Given a Jira ticket and a GitHub repository, the system fetches the story, studies the relevant codebase, identifies where changes are needed, and produces structured technical subtasks.

The goal is not only to summarize a Jira ticket, but to connect the story to real files, APIs, models, schemas, components, and tests in the target repository.

## Multi-Agent Workflow

The pipeline is organized as specialized agents connected through a LangGraph state graph:

```text
jira_node -> github_node -> planner_node -> create_subtasks_node
```

Each node receives and updates shared graph state, allowing later steps to build on earlier outputs.

## Agent Responsibilities

### Jira Agent

The Jira agent uses Atlassian MCP tools to fetch the requested Jira story. It formats the story into structured Markdown with fields such as title, issue key, status, description, acceptance criteria, comments, and related metadata.

### GitHub Agent

The GitHub agent uses GitHub MCP tools to inspect the repository. It reads project context, searches relevant files, and maps Jira requirements to actual code areas. Its output focuses only on codebase-related implementation context.

### Planner Node

The planner combines the Jira story and GitHub codebase context into actionable technical subtasks. It avoids overly small task splits and groups closely related implementation work into meaningful chunks.

### Jira Subtask Agent

The subtask creation agent creates Jira subtasks under the parent story. It uses the planner output as the source of truth and preserves each planned subtask's title, details, files or areas, and acceptance check.

## Key Concepts

- `GraphState` carries ticket ID, Jira details, repository context, planned subtasks, and created subtask results.
- MCP tool allowlists limit each agent to only the tools it needs.
- ReAct agents are used where external Jira or GitHub tool calls are required.
- The pipeline uses Moonshot AI Kimi K2.5 through AWS Bedrock as the LLM.
- The planner node uses the LLM directly to synthesize implementation subtasks.
- Prompts are intentionally strict to reduce hallucinated files, invented requirements, and unnecessary tool usage.

## Primary Output

The system produces a structured implementation breakdown:

```text
- Subtask: <short action-oriented title>
  Details: <specific implementation work>
  Files/Areas: <relevant files or code areas>
  Acceptance Check: <how to verify completion>
```

The final output also includes the Jira subtask keys created under the parent story.
