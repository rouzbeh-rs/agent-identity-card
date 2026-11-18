# Agent White Paper Protocol (AWPP) — Proof-of-Concept Repository

## Overview

This repository accompanies the paper **"Agent White Paper Protocol: A Standardized Disclosure Format for Autonomous Agents Inspired by the Cross-Domain Nutrition Label Paradigm."**

The paper proposes the first standardized human-facing disclosure format for autonomous AI agents. Its central design contribution is the **Agent Identity Card** — a fixed-format, one-page disclosure whose design is grounded in the empirically validated "standardized disclosure pattern," a cross-domain paradigm with independent validation in:

- **Food labeling** — FDA Nutrition Facts (1990, redesigned 2016)
- **Energy efficiency** — EU Energy Labels (1994, rescaled 2021)
- **Digital privacy** — Privacy nutrition labels (Kelley et al. 2009, 2010)
- **App store transparency** — Apple/Google Privacy Labels (2020–2022)
- **Cybersecurity** — IoT security and privacy labels (Emami-Naeini et al. 2019–2023)
- **Pharmaceuticals** — FDA Drug Facts Label (2002)

The paper extracts **eleven design principles** from this cross-domain survey and evaluates **five alternative card designs** before justifying the chosen format as a principled synthesis.

## Repository Contents

```
├── agent-identity-extension/     # Chrome extension (Manifest V3)
│   ├── content.js                # URL matcher + card renderer
│   ├── card.css                  # Identity Card styling
│   ├── manifest.json             # Extension manifest
│   ├── icons/                    # Extension icons
│   └── data/                     # Tier 1 JSON data models
│       ├── perplexity.json       # Perplexity AI audit
│       ├── claude.json           # Claude (Anthropic) audit
│       └── chatgpt.json         # ChatGPT (OpenAI) audit
└── figures/                      # Paper figures
    ├── fig1a_variant_a.png       # Variant A: Composite Letter Grade
    ├── fig1b_variant_b.png       # Variant B: Grid/Icon Layout
    ├── fig1c_variant_c.png       # Variant C: Risk-Stratified Layers
    ├── fig1d_variant_d.png       # Variant D: Risk-First Format
    ├── fig1e_variant_e.png       # Variant E: Progressive Disclosure
    ├── fig1f_chosen_design.png   # Chosen Design: Principled Synthesis
    └── fig2_identity_cards.png   # Three rendered Identity Cards
```

## Browser Extension

The browser extension is a proof-of-concept that renders Agent Identity Cards for three commercially deployed agents. It demonstrates the protocol's two-tiered architecture:

- **Tier 1 (Technical White Paper):** A prescriptive JSON schema serving as the machine-readable data model
- **Tier 2 (Agent Identity Card):** A standardized, human-readable disclosure generated from that data model

### Architecture

```
┌─────────────────────────────────────────┐
│  Browser Extension (Client-Side)        │
│                                         │
│  ┌─────────────┐   ┌────────────────┐   │
│  │ URL Matcher │──>│ Mock Registry  │   │
│  │ (content.js)│   │ (data/*.json)  │   │
│  └──────┬──────┘   └───────┬────────┘   │
│         │                  │            │
│         v                  v            │
│  ┌─────────────────────────────────┐    │
│  │  Identity Card Renderer         │    │
│  │  (HTML/CSS DOM Injection)       │    │
│  └─────────────────────────────────┘    │
└─────────────────────────────────────────┘
```

The extension does NOT use an LLM, does NOT scrape websites, and does NOT infer capabilities. All data comes from pre-authored JSON files created through manual auditing of publicly available documentation.

### Target Agents

| Agent | URL | JSON File | Transparency |
|-------|-----|-----------|-------------|
| Perplexity AI | perplexity.ai | `data/perplexity.json` | Moderate (83%) |
| Claude (Anthropic) | claude.ai | `data/claude.json` | High (94%) |
| ChatGPT (OpenAI) | chatgpt.com | `data/chatgpt.json` | Moderate (89%) |

### Installation (Chrome/Edge)

1. Download or clone this repository.
2. Open Chrome and navigate to `chrome://extensions/`.
3. Enable **Developer mode** (toggle in the top-right corner).
4. Click **Load unpacked** and select the `agent-identity-extension` folder.
5. Navigate to any of the target agent URLs listed above.
6. Click the blue **ⓘ** button in the bottom-right corner to view the Identity Card.

## Five Design Variants

The paper evaluates five alternative Identity Card designs, each drawing from a different disclosure domain:

| Variant | Model | Decision |
|---------|-------|----------|
| **A: Composite Letter Grade** | EU Energy Label | Rejected — loses per-section granularity |
| **B: Grid/Icon Layout** | Apple Privacy Labels | Partially adopted — icons for capabilities, prose for architecture |
| **C: Risk-Stratified Layers** | IoT Security Labels | Adopted at macro level (Card→JSON) only |
| **D: Risk-First Format** | FDA Drug Facts | Partially adopted — mandatory "Known Limitations" |
| **E: Progressive Disclosure** | HCI (Nielsen) | Rejected within card — hidden info stays hidden |

Each variant is documented as a separate figure (`figures/fig1a` through `fig1f`). The chosen design (`fig1f`) is a principled synthesis. Figure 2 (`fig2_identity_cards.png`) shows the three rendered Agent Identity Cards.

## JSON Data Model (Tier 1 Schema)

Each JSON file follows the AWPP schema with six mandatory sections:

- **Identity & Provenance**: Name, version, legal operator, license, DID
- **Purpose & Constraints**: Task description, intended use, out-of-scope use
- **Architecture**: Base model, planning module, memory system, knowledge system
- **Capabilities & Tools**: Enumerated tools and API access
- **Communication & Interoperability**: I/O modalities, protocol support
- **Evaluation & Benchmarks**: Performance records and known limitations

Each file also includes an `_auditMetadata` section documenting the audit process: which fields could not be found, difficulty ratings per section, and methodology notes.

## Key Findings (Case Study)

- **11% non-disclosure rate** across 54 audited fields (3 agents × 18 fields)
- **Structurally uneven gap**: Identity & Purpose = 100% disclosed; Architecture & Evaluation = 56–67% verified
- **Capability vocabulary problem**: Inconsistent naming (webSearch vs. webBrowsing) defeats machine comparability
- These findings validate specific design choices (e.g., rejecting the composite letter grade that would mask per-section asymmetry)

## Customizing for Your Audit

To add a new agent or refine existing data:

1. Copy an existing JSON file in `data/` as a template.
2. Update all fields based on your documentation audit.
3. Fill in the `_auditMetadata` section with your audit findings.
4. Add the new hostname to the `AGENT_REGISTRY` map in `content.js`.
5. Add the URL pattern to `manifest.json` under both `content_scripts.matches` and `web_accessible_resources.matches`.

## Important Notes

- This is a **research prototype**, not a production tool.
- Data is sourced from **public documentation** and has NOT been verified by the agent developers.
- In a fully deployed AWPP ecosystem, developers would author and cryptographically sign their own JSON data models. This extension simulates that by using researcher-authored data.
- The proof-of-concept card deliberately **omits verification iconography**, since researcher-authored data lacks the cryptographic signatures those icons would represent.
- The `_auditMetadata.fieldsNotDisclosed` array explicitly tracks which schema fields could not be populated — this transparency gap is itself a key finding of the case study.

## License

This extension is released for academic research purposes under the MIT License.

## Citation

If you use this extension or data in your research, please cite the Agent White Paper Protocol paper.
