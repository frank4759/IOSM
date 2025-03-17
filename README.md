# IOSM v1.0

[![Validate Spec Repository](https://github.com/rokoss21/IOSM/actions/workflows/validate-spec-repo.yml/badge.svg)](https://github.com/rokoss21/IOSM/actions/workflows/validate-spec-repo.yml)
![Spec Version](https://img.shields.io/badge/spec-v1.0-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Repository Type](https://img.shields.io/badge/repository-specification-orange.svg)

**IOSM (Improve -> Optimize -> Shrink -> Modularize)** is an engineering methodology for continuous system improvement. This repository is the **canonical specification repository** for IOSM v1.0 and contains the normative spec, machine-readable schemas, and example artifacts required to understand, adopt, and validate the methodology.

> **Looking for the runtime?** See [**iosm-cli**](https://github.com/rokoss21/iosm-cli) — a terminal coding agent that implements IOSM as an engineering runtime with orchestration, MCP, checkpoints, and extensions.

## What This Repository Contains

- The normative IOSM v1.0 specification: [IOSM_SPEC_v1.0.md](IOSM_SPEC_v1.0.md)
- Canonical example artifacts in [artifacts/examples](artifacts/examples)
- Reusable artifact templates in [artifacts/templates](artifacts/templates)
- JSON Schemas in [schemas](schemas)
- A reference repository validator in [scripts/validate_repository.py](scripts/validate_repository.py)
- Contribution guidance in [CONTRIBUTING.md](CONTRIBUTING.md)
- Version history in [CHANGELOG.md](CHANGELOG.md)

## Start Here

If you are new to IOSM, read the repository in this order:

1. This `README.md`
2. [IOSM_SPEC_v1.0.md](IOSM_SPEC_v1.0.md)
3. [artifacts/README.md](artifacts/README.md)
4. The example artifacts in [artifacts/examples](artifacts/examples)
5. The machine-readable schemas in [schemas](schemas)
6. The validation entrypoint in [scripts/validate_repository.py](scripts/validate_repository.py)

## Repository Layout

```text
.
├── README.md
├── IOSM_SPEC_v1.0.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE
├── Makefile
├── requirements-dev.txt
├── artifacts/
│   ├── README.md
│   ├── examples/
│   └── templates/
├── schemas/
├── scripts/
└── .github/
    └── workflows/
```

## Core Repository Documents

| File | Purpose |
| --- | --- |
| [IOSM_SPEC_v1.0.md](IOSM_SPEC_v1.0.md) | Normative specification and execution model |
| [artifacts/README.md](artifacts/README.md) | Artifact map and usage guide |
| [schemas/iosm.schema.json](schemas/iosm.schema.json) | JSON Schema for the IOSM configuration model |
| [schemas/hypothesis-card.schema.json](schemas/hypothesis-card.schema.json) | JSON Schema for Hypothesis Cards |
| [schemas/cycle-report.schema.json](schemas/cycle-report.schema.json) | JSON Schema for Cycle Reports |
| [scripts/validate_repository.py](scripts/validate_repository.py) | Reference validator for repository consistency and artifact conformance |

## Canonical Artifact Set

IOSM v1.0 expects the following artifact classes in a conformant implementation:

- `iosm.yaml`
- Baseline Report
- Hypothesis Card Set
- Invariant Catalog
- Phase Report
- Cycle Report
- Metrics History
- Waiver Register
- Contract Catalog
- Decision Log
- Pattern Library

This repository ships both **examples** and **templates** for these artifacts under [artifacts/](artifacts/).

## IOSM Execution Model

```text
Codebase
   ↓
Baseline Report          ← measure current state
   ↓
Hypothesis Cards         ← define what to change and why
   ↓
IOSM Phases
   Improve → Gate_I
   Optimize → Gate_O
   Shrink → Gate_S
   Modularize → Gate_M
   ↓
Cycle Report             ← record what happened
   ↓
Metrics History          ← track evolution over time
   ↓
Decision Log             ← preserve rationale
```

Each phase has a quality gate. If a gate fails, the cycle can repeat or adjust hypotheses.

## Example: Real IOSM Cycle

```text
Service: auth-gateway
Cycle:   2026-03-10-001

Baseline:
  complexity index   0.63
  simplicity         0.51
  modularity         0.47

Hypotheses:
  H1  reduce duplicated validation logic across 3 middleware layers
  H2  isolate token refresh into a standalone module

Result:
  simplicity         0.65  (+14%)
  modularity         0.56  (+9%)
  performance        0.82  (+6%)
  IOSM Index         0.71  (+0.12)

Artifacts:
  .iosm/cycles/2026-03-10-001/baseline-report.json
  .iosm/cycles/2026-03-10-001/hypotheses.json
  .iosm/cycles/2026-03-10-001/cycle-report.json
  .iosm/cycles/2026-03-10-001/phase-reports/improve.json
```

## Spec → Runtime Mapping

How IOSM spec artifacts map to [iosm-cli](https://github.com/rokoss21/iosm-cli) implementation:

| IOSM Spec Artifact | iosm-cli Path | Created By |
|--------------------|---------------|------------|
| `iosm.yaml` | `iosm.yaml` | `/init` |
| Baseline Report | `.iosm/cycles/*/baseline-report.json` | `/iosm` (auto) |
| Hypothesis Cards | `.iosm/cycles/*/hypotheses.json` | `/iosm` (auto) |
| Phase Reports | `.iosm/cycles/*/phase-reports/*.json` | `/iosm` (per phase) |
| Cycle Report | `.iosm/cycles/*/cycle-report.json` | `/iosm` (on completion) |
| Metrics History | `.iosm/metrics-history.jsonl` | `/iosm` (append) |
| Decision Log | `.iosm/decision-log.md` | `/iosm` + manual |
| Pattern Library | `.iosm/pattern-library.md` | `/iosm` + manual |
| Invariant Catalog | `.iosm/invariants.yaml` | `/init` |
| Contract Catalog | `.iosm/contracts.yaml` | `/init` |
| Waiver Register | `.iosm/waivers.yaml` | manual |

## Schemas and Validation

The schemas in [schemas/](schemas/) are intended to make IOSM machine-readable and validator-friendly.

- Use [schemas/iosm.schema.json](schemas/iosm.schema.json) to validate configuration files.
- Use [schemas/hypothesis-card.schema.json](schemas/hypothesis-card.schema.json) to validate change hypotheses.
- Use [schemas/cycle-report.schema.json](schemas/cycle-report.schema.json) to validate cycle outputs and reporting pipelines.

The schemas are intentionally conservative in v1.0: they validate the repository's canonical fields without forcing one implementation language or storage model.

## Local Validation

Install the development dependencies and run the repository validator:

```bash
python3 -m pip install -r requirements-dev.txt
make validate
```

You can also invoke the validator directly:

```bash
python3 scripts/validate_repository.py
```

The validator checks required repository files, schema integrity, example-to-schema conformance, index-weight consistency, and broken local Markdown links.

## Intended Audience

IOSM is intended for:

- platform engineering teams
- architecture and governance functions
- reliability and performance teams
- teams building CI/CD policy and validation systems
- AI and automation engineers building governed engineering agents

## Adoption Model

IOSM can be adopted incrementally:

1. Start with the spec and examples.
2. Define your `iosm.yaml` from the canonical schema.
3. Implement the required artifacts for one service or subsystem.
4. Automate gate evaluation and reporting.
5. Introduce confidence scoring, guardrails, and stabilization rules.
6. Scale to portfolio comparison once delivery boundaries and evidence quality are stable.

## Versioning

- The current normative document is [IOSM_SPEC_v1.0.md](IOSM_SPEC_v1.0.md).
- Future incompatible spec versions should be introduced as new versioned files, for example `IOSM_SPEC_v1.1.md` or `IOSM_SPEC_v2.0.md`.
- Repository-level changes are tracked in [CHANGELOG.md](CHANGELOG.md).

## CLI Implementation

Use [**iosm-cli**](https://github.com/rokoss21/iosm-cli) when you want to execute the spec operationally rather than adopt it only as documentation. It is the reference runtime that implements this specification as a terminal coding agent.

```bash
npm install -g iosm-cli
iosm
```

| Feature | Description |
|---------|-------------|
| IOSM cycles | `/init` + `/iosm` — measurable improve/verify loops with artifact output |
| Orchestration | `/orchestrate` — parallel/sequential agents with dependencies, locks, worktrees |
| Operational controls | `/checkpoint`, `/rollback`, `/doctor`, `/memory` |
| Extensibility | MCP servers, TypeScript extensions, SDK, JSON/RPC modes |
| 15+ LLM providers | Anthropic, OpenAI, Gemini, Groq, xAI, OpenRouter, Mistral, and more |

[![npm version](https://img.shields.io/npm/v/iosm-cli?style=flat-square&color=cb3837)](https://www.npmjs.com/package/iosm-cli)

## Current Status

This repository is a **specification repository with automated validation**.

What is already present:

- a full normative specification;
- canonical examples and templates;
- machine-readable schemas for the primary artifacts;
- a reference repository validator;
- CI automation for repository validation;
- a reference CLI implementation: [iosm-cli](https://github.com/rokoss21/iosm-cli).

## Roadmap

1. Publish worked case studies.
2. Add visualization guidance for radar charts, index timelines, and drift heatmaps.
3. Expand the implementation toolkit on top of the repository validator.
4. Add additional schemas and conformance fixtures for future spec versions.
5. Deepen integration with [iosm-cli](https://github.com/rokoss21/iosm-cli) runtime.

## Contributing

Contributions are welcome, especially if they improve:

- clarity of the normative spec;
- schemas and machine-readable artifacts;
- examples and templates;
- interoperability guidance;
- case-study quality.

Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

## Author

**Emil Rokossovskiy**

- GitHub: [@rokoss21](https://github.com/rokoss21)
- Email: [ecsiar@gmail.com](mailto:ecsiar@gmail.com)

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
