# Premodern Jewish Research Skill for Claude

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A specialized Claude Skill for rigorous research into Jewish texts and topics using **premodern primary sources** (pre-Haskalah, roughly before 1800). It prioritizes direct engagement with classical commentators in their original languages (Hebrew, Aramaic, etc.), preserves disagreements among authorities, and provides historical/biographical context for each cited voice. It heavily leverages the capabilities of connected texts at sefaria.org.

This skill is ideal for:
- Torah commentary and parshanut
- Halakhic (legal) questions
- Theological or philosophical inquiry
- Textual interpretation grounded in Rishonim and early Acharonim

It deliberately avoids modern denominational lenses, secondary summaries, or post-1800 reactions to Enlightenment/denominationalism.

## Core Features

- **Source-first approach** — Works directly from primary texts via Sefaria.org and similar databases
- **Original-language analysis** — Performs interpretive work in Hebrew/Aramaic before translating
- **Preserves machloket** — Presents multiple legitimate views with the principle "These and these are the words of the living God"
- **Historical context** — Includes material circumstances, geographic/political setting, and social position of each commentator
- **Two modes**:
  - **Research Mode** (default): Sources + context + disagreements with minimal synthesis
  - **Synthesis Mode**: Connects patterns across texts when user prompts for deeper exploration
- **Bias countermeasures** — Explicitly deprioritizes modern/denominational overreach and recency bias in searches

## Installation

1. **Clone or download** this repository:
   ```bash
   git clone https://github.com/ChelmsDeep/premodern-jewish-research-skill.git
   # or download ZIP from the repo page
