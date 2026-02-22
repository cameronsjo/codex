---
name: license-advisor
description: Interview-driven license selection for projects. Asks about goals, audience, and monetization risk, then recommends a license with rationale.
---

Help the user choose the right software license for a project through a structured interview, scoping analysis, and tailored recommendation.

## Default Posture

The user's default stance: **ideas are worth sharing, but others should not profit from them without contributing back.** Weight recommendations accordingly — permissive licenses (MIT, BSD, ISC) require explicit justification.

## Phase 1: Interview

Use AskUserQuestion to gather context. Ask up to 3 rounds — stop early if you have enough signal.

### Round 1 — Project Identity

Ask these two questions together:

1. **What kind of project is this?**
   - Library/SDK (consumed as a dependency)
   - Application/Tool (run directly by users)
   - Framework (others build on top of)
   - Infrastructure/Platform (deployed as a service)

2. **Who is the audience?**
   - Personal use / portfolio project
   - Open source community
   - Enterprise / commercial users
   - Mixed (community + commercial)

### Round 2 — Monetization & Risk

Ask these two questions together:

1. **Could this become a product or service?**
   - No — purely open, no commercial intent
   - Maybe — want to preserve the option
   - Yes — planning to monetize eventually
   - Already monetized

2. **What's the biggest risk you want to protect against?**
   - Cloud provider wraps it as a managed service (the AWS problem)
   - Competitor forks, closes source, and competes (the Android problem)
   - Large company uses it without contributing back (the free-rider problem)
   - None — just want it out there

### Round 3 — Ecosystem (only if needed)

Ask if the answers so far don't clearly point to a recommendation:

1. **Will you accept outside contributors?**
   - Solo project, no outside PRs
   - Open to contributions
   - Actively seeking contributors / community-driven

2. **Are there dependency constraints?**
   - Check if key dependencies use copyleft licenses that would propagate
   - Check if the project will be consumed by copyleft-incompatible ecosystems

## Phase 2: Scope Analysis

After the interview, analyze the project against these dimensions. Present this as a brief table or summary — do not skip this step.

| Dimension | Assessment |
|---|---|
| **Monetization surface** | How likely is commercial value? (SaaS potential, consulting, dual-license) |
| **Free-rider risk** | Could a well-funded competitor extract value without contributing? |
| **Adoption friction** | Will the license slow adoption in the target audience? |
| **Contributor dynamics** | Solo vs community — does CLA/DCO matter? |
| **Dependency compatibility** | Any license conflicts with upstream or downstream? |

## Phase 3: Recommendation

Present a single recommended license with:

1. **The recommendation** — license name and SPDX identifier
2. **Why this one** — 2-3 sentences connecting the interview answers to the choice
3. **What it protects** — the specific risk it mitigates
4. **What it costs** — any adoption friction or ecosystem limitations
5. **Monetization path** — how to make money with this license if relevant

Then show a comparison table of the top 2-3 candidates:

| License | Protects Against | Adoption Friction | Monetization Path |
|---|---|---|---|
| Recommended | ... | ... | ... |
| Alternative 1 | ... | ... | ... |
| Alternative 2 | ... | ... | ... |

### License Quick Reference

Use this to inform recommendations — do NOT just dump this table on the user.

| License | SPDX | Type | Copyleft | SaaS Protection | Monetization |
|---|---|---|---|---|---|
| MIT | MIT | Permissive | No | No | None — anyone can commercialize |
| Apache 2.0 | Apache-2.0 | Permissive | No | No | Patent grant, but no SaaS protection |
| AGPL-3.0 | AGPL-3.0-only | Copyleft | Strong (network) | Yes | Dual-license (AGPL + commercial) |
| GPL-3.0 | GPL-3.0-only | Copyleft | Strong | No (SaaS loophole) | Dual-license |
| LGPL-3.0 | LGPL-3.0-only | Weak copyleft | Linking exception | No | Libraries consumed by proprietary code |
| MPL-2.0 | MPL-2.0 | File-level copyleft | Per-file | No | Middle ground — file-level sharing |
| BSL 1.1 | BUSL-1.1 | Source-available | N/A | Yes | Time-delayed open source (change date) |
| FSL 1.1 | (non-SPDX) | Source-available | N/A | Yes | Converts to Apache 2.0 after 2 years |
| ELv2 | (non-SPDX) | Source-available | N/A | Yes | Prevents managed service competition |
| Unlicense | Unlicense | Public domain | No | No | None — full relinquishment |

### CLA / DCO Advisory

If the recommendation is copyleft and the user plans to accept contributors:

- **Suggest a CLA** if they want to preserve dual-licensing rights (sole copyright holder can offer commercial licenses)
- **Suggest a DCO** (Developer Certificate of Origin) if they want lightweight contribution tracking without legal overhead
- Explain the trade-off: CLA preserves maximum flexibility but adds contributor friction; DCO is lighter but doesn't grant relicensing rights

## Phase 4: Apply (if requested)

If the user asks to apply the license:

1. Fetch the license text from a canonical source (e.g., `https://www.gnu.org/licenses/` or `https://opensource.org/licenses/`)
2. Write to `LICENSE` in the project root
3. Update `package.json`, `pyproject.toml`, `Cargo.toml`, or equivalent with the SPDX identifier
4. Update `README.md` license section if one exists
5. Add SPDX license header to source files if the license recommends it (AGPL, GPL, MPL)

## Important

- Never recommend MIT/BSD/ISC without the user explicitly saying they want maximum permissiveness
- Always surface the "MIT regret" pattern when permissive licenses are considered for projects with commercial potential
- The SPDX identifier matters — use `-only` suffix for GPL/AGPL/LGPL to prevent "or later" ambiguity
- Source-available licenses (BSL, FSL, ELv2) are NOT open source — always flag this distinction
- License changes after public release with external contributors are effectively impossible without a CLA
