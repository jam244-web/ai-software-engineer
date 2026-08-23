# AI Software Engineer

An autonomous software engineering system built with **LangGraph**, **OpenAI**, and the **Model Context Protocol (MCP)**.

The system accepts a software development task, generates an implementation plan, writes production-ready code, reviews its own work, executes the generated project, detects runtime failures, repairs the responsible files, and repeats until the project successfully executes.

---

## Features

- Multi-agent architecture
- LangGraph orchestration
- Structured LLM outputs
- Automatic planning
- Code generation
- Code review with retry loops
- Runtime execution
- Execution-aware repair loop
- MCP filesystem integration
- Stateful workflow execution

---

## Architecture

The system consists of six specialized agents:

| Agent | Responsibility |
|--------|----------------|
| Planner | Breaks a software task into implementation steps |
| Coder | Generates production-ready code for a single step |
| Reviewer | Reviews generated code and requests revisions if necessary |
| Filesystem | Writes approved files using MCP |
| Executor | Executes the generated project (install, test, run) |
| Execution Reviewer | Analyses runtime failures and identifies the file responsible |

---

## Workflow

1. User provides a software engineering task.
2. Planner creates an implementation plan.
3. Coder implements the current step.
4. Reviewer validates generated code.
5. Failed reviews are sent back to the coder.
6. Approved files are written through MCP.
7. After all files are generated, the executor:
   - installs dependencies
   - runs automated tests
   - validates application startup
8. If execution fails:
   - an Execution Reviewer analyses the failure
   - identifies the file responsible
   - sends only that file back for repair
9. The workflow repeats until execution succeeds.

---

## Technologies

- Python
- LangGraph
- LangChain
- OpenAI Structured Outputs
- Pydantic
- FastAPI
- MCP (Filesystem Server)

---

## Example Repair Loop

```
Execution Failed

↓

pytest reports failing tests

↓

Execution Reviewer

↓

Target file:
test_main.py

↓

Coder regenerates test_main.py

↓

Reviewer approves

↓

File written via MCP

↓

Tests rerun

↓

Success
```

---

## Project Structure

```
backend/
│
├── agents/
│   ├── planner.py
│   ├── coder.py
│   ├── reviewer.py
│   ├── filesystem.py
│   ├── executor.py
│   └── execution_reviewer.py
│
├── graphs/
│
├── prompts/
│
├── services/
│
├── models/
│
└── generated_project/
```

---

## Future Improvements

- Docker execution
- Ruff / Black / MyPy validation
- Git integration
- Parallel execution
- Dependency-aware execution
- Incremental builds
- Web interface for workflow visualization

---

## Why this project?

Most code-generation agents stop after producing source code.

This project continues until the generated software **actually executes successfully**, creating an autonomous software engineering workflow rather than a simple code generator.
