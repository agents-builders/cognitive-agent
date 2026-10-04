# Cognitive Agent

> From chatbots to proactive, persistent agents.

[![Python 3.11+](https://img.shields.io/badge/python-3.11%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**Cognitive Agent** is a Python framework for building *layered cognitive agents* — agents with a real cognitive architecture (memory, planning, reflection, tool use), not just an LLM wrapped in a chat loop.

Most agent frameworks give you a prompt loop and call it an agent. This project starts from a different premise: durable, trustworthy agency needs **structure** — separated concerns, explicit contracts between layers, and cognition you can inspect, test, and evolve.

## Why another agent framework?

- **Persistence over prompts.** Agents that keep memory, state, and goals across sessions — not goldfish chatbots that forget everything on restart.
- **Proactivity over reactivity.** The architecture supports agents that act on their own initiative (hooks, schedulers, standing goals) — not just responders waiting for input.
- **Engineering discipline over demos.** Strict layering (L0–L4), an architecture decision record (ADR) for every major choice, contract tests, and import-linter-enforced boundaries. Built to be maintained, not just demoed.

## Architecture

Five layers with strict one-way dependencies — a lower layer never depends on an upper one:

| Layer | Responsibility | Examples |
|---|---|---|
| L4 Application / Orchestration | Minimal developer API; the composition root — the only place allowed to reference all lower layers for DI assembly | `Agent`, `Team`, `TeamLead` |
| L3 Agent Abstraction | Agent lifecycle & team orchestration | `BaseAgent`, `Supervisor`, `TeamOrchestrator` |
| L2 Cognitive Runtime | The core loop | `CognitiveRuntime`, `StrategyRegistry`, Hooks |
| L1 Cognitive Components | Independently testable cognitive modules | Brain / Body / Memory / EventBus |
| L0 Infrastructure | LLM adapters, tool protocols, state management | `LLMAdapter`, `ToolProtocol`, `StateStore` |

The runtime kernel lives in `lca_kernel/`; the framework itself in `lca/`. The full rationale is recorded in [ADR-0001](docs/adr/0001-five-layer-separation.md).

## Quickstart

Requires Python 3.11+.

```bash
git clone https://github.com/agents-builders/cognitive-agent.git
cd cognitive-agent
pip install -e .   # or: uv sync
```

```python
from lca import Agent, Team  # facade API, resolved lazily

# See docs/ and AGENTS.md for the current API surface and examples —
# the framework is under active development.
```

Run the test suite:

```bash
pytest tests/ -q
```

## Documentation

| You are looking for | Where to go |
|---|---|
| Architecture Decision Records (index) | [docs/adr/README.md](docs/adr/README.md) |
| Repository contract for coding agents (read first) | [AGENTS.md](AGENTS.md) |
| Documentation map | [docs/specs/documentation-map.md](docs/specs/documentation-map.md) |
| Engineering guardrails | [docs/agent-contract/coding-guardrails.md](docs/agent-contract/coding-guardrails.md) |
| Framework source | `lca/` · runtime kernel `lca_kernel/` |
| Tests | `tests/` |

Every significant design choice in this repo has a corresponding ADR under `docs/adr/`. If you're wondering *why* something is built the way it is, start there.

## Project status

Under active development. APIs may change; ADRs are the source of truth for design intent, and the test suite is the source of truth for behavior.

## Contributing

Issues and pull requests are welcome. Please read [AGENTS.md](AGENTS.md) first — it documents the layering rules, commit conventions, and quality bars (including: no test watering-down, green tests as the definition of done).

## License

MIT — see [LICENSE](LICENSE).
