# Atlassian Systems Analyst

An AI agent skill for systems analysts working with **Atlassian Jira and Confluence**, with PostgreSQL as a source of supporting data. It collects context through MCP and produces a detailed `task.md` with requirements, source references, open questions, and PlantUML diagrams.

Skill identifier: `atlassian-analyst`.

## What it does

- Retrieves the issue list in a separate Jira query, then reads issue details, comments, relationships, and attachment metadata.
- Collects linked and relevant Confluence pages, including available tables, code, and document metadata.
- Inspects relevant PostgreSQL schemas and runs scoped, read-only `SELECT` queries to check facts against the data.
- Separates confirmed facts from assumptions and highlights conflicting sources.
- Produces implementation requirements, acceptance criteria, a requirements-to-source traceability table, and process and data diagrams.
- Records missing permissions, truncated results, unavailable attachments, and other gaps explicitly.

## Requirements

Use an agent environment that supports `SKILL.md`, MCP tools, and writing local files. Connect MCP tools for Jira, Confluence, and PostgreSQL, with access to the relevant project, documentation, and database.

The workflow attempts all three sources. If a source is unavailable, the analysis must report the missing context. This repository supplies instructions; it does not install MCP servers or provide credentials.

## Installation

Clone the repository into a folder named after the skill:

```sh
git clone https://github.com/MorevPro/atlassian-analyst-skill.git atlassian-analyst
```

Register or copy that folder into your agent's skill location according to its installation instructions. Keep `SKILL.md` at the root of the skill folder.

To update a Git-based installation:

```sh
git -C atlassian-analyst pull --ff-only
```

For a ZIP installation, extract the `atlassian-analyst` folder and install it through your agent's supported skill workflow.

## Usage

Ask the agent:

```text
Use the atlassian-analyst skill for Jira issue ABC-123. Call the connected
Jira, Confluence, and PostgreSQL MCP tools. Retrieve the related issue list
in a separate Jira call, then collect context from each source. Prepare
a task.md with source material, systems analysis, and PlantUML diagrams.
Write the report in English.
```

You can supply a JQL query or filter instead of a single issue key, and specify an output path.

Default report locations:

- Single issue: `~/analyst-tasks/<JIRA_KEY>/task.md`
- Query or filter: `~/analyst-tasks/query/task.md`

These existing output paths are preserved for compatibility. The main deliverable is `task.md`; the current skill also calls for links to separate `.puml` diagram files where used.

## Language and limitations

This README and the discovery metadata are in English. The detailed instructions in `SKILL.md` are currently in Russian. Specify the desired report language in your request.

The skill is designed to read Jira, Confluence, and PostgreSQL without modifying those sources. It masks secrets found in source material and treats retrieved content as data rather than instructions.

Accuracy and completeness depend on MCP permissions and the selected scope. Database contents describe the observed state; they do not automatically establish a business requirement. If a source truncates a response, an attachment cannot be read, or data changes during collection, the report must disclose that limitation. PlantUML rendering is checked only when a renderer is available.

This is an independent community skill and is not affiliated with or endorsed by Atlassian.
