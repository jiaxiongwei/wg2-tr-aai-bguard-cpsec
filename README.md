# TR.aai-bguard-cpsec

**Analysis of behavioural guardrails for agentic AI: safety and security for
cyber-physical systems**

Proposed new Technical Report of the Focus Group on Trust and Identity for
humans and agentic AI (FG-TIDA), Working Group 2 (WG2). Status: **draft (V1)**.

📄 **Web page:** https://jiaxiongwei.github.io/wg2-tr-aai-bguard-cpsec/
📥 **PDF:** https://jiaxiongwei.github.io/wg2-tr-aai-bguard-cpsec/specification.pdf

## What this document is

Agentic AI is moving from generating content to taking actions in the physical
world. When an autonomous agent controls a cyber-physical system — a robot, a
vehicle, an industrial plant — its actions carry consequences that are often
irreversible, and a guardrail that is probabilistic or too slow to meet a
control deadline is effectively useless. This Technical Report analyses
behavioural guardrails for agentic AI from the system side: the object of
protection is the external cyber-physical system, not the agent, its model or
its provider's infrastructure.

It separates **behavioural safety** — harm caused by an agent that is not under
adversarial control — from **behavioural security** — harm caused when an
adversary compromises the agent, its tools or its supply chain and turns its
actions against the plant. It classifies the attack surfaces, examines how
guardrail mechanisms work and where they fail, and proposes a general,
vendor-neutral assessment framework covering enforcement layers, trust
boundaries and integration with the functional safety case, including
deterministic, in-deadline and fail-closed enforcement. It closes with the
elements that appear standardizable. It does not address protection of the agent
itself, except where that protection bounds the risk the agent poses.

## Contents

| | |
|---|---|
| 1–4 | Scope, References, Definitions, Abbreviations |
| 5 | Conventions (`None.`) |
| 6–8 | Overview, Current state, Development trends |
| 9 | Technical analysis |
| 10 | System-facing behavioural risk taxonomy (RSK-01 – RSK-16) |
| 11 | Assessment factors and collection methods (AF-01 – AF-10) |
| 12 | A general assessment framework for behavioural guardrails in cyber-physical systems |
| 13–14 | Standardization points, Conclusion |
| Appendix I–IV | Key terms, guardrail mapping, documented events, existing instruments |
| Bibliography, Annex A | References and the A.1 justification form |

## Working on the document

The document is authored in reStructuredText and built with Sphinx into a web
page and a print PDF.

```shell
pip install -r requirements.txt
sphinx-build -b html src/doc target/html                            # web
sphinx-build -b latex src/doc target/latex && make -C target/latex  # PDF
```

`target/` is a **local build directory only** — it is git-ignored and never
published. The public site and PDF are built by GitHub Actions on every push to
`main` and deployed to the addresses above.

- `src/doc/index.rst` — title, abstract, background and proposal, summary, history,
  authors, keywords, foreword, note.
- `src/doc/chapters/` — one file per clause, numbered `01`–`14`, then the four
  appendices, the bibliography and the Annex A justification form.
- `src/doc/images/` — figures.
- `src/doc/conf.py` — project metadata and the `cover_*` values used on the PDF cover.

See **[CONTRIBUTING.md](CONTRIBUTING.md)** for how to propose a change, and
**[WG-ONBOARDING.md](WG-ONBOARDING.md)** for the TIDA authoring workflow.

## Editor

Xiongwei Jia, China Unicom — jiaxw9@chinaunicom.cn

## Licence

MIT, per [LICENSE](LICENSE). See [CHANGELOG.md](CHANGELOG.md) for document history.
