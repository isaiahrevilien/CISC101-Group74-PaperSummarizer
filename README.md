# 📘 Paper Summarizer — CISC101 Group

This repository contains the full implementation of the Paper Summarizer project, following the modular architecture pattern used in the Travel Planner assignment.
The system processes full academic papers and produces structured summaries, section tables, glossaries, and warnings based on the PS2 Specification.

## 📂 Repository Structure/
 │ system_prompt.md
 │ README.md
 │
 └── modules/
       module1_intake_setup.md
       module2_section_loop.md
       module3_guardrails.md
       module4_rendering_refinement.md
       module5_citation_extraction_verification.md
       module6_key_contributions_rhetorical_moves.md

## 🧠 System Prompt

system_prompt.md includes:

Greeting & setup instructions

Required user inputs (paper text, section list, audience)

Boundaries & safety rules (no hallucination, no fabricated citations, no reordering)

Required outputs

MLA & word-limit enforcement

Embedded PS2 Specification

## 🔧 Modules
### ⚙️ Module 1: Intake & Setup

Normalize section titles.

Detect missing or empty sections.

Enforce MLA expectations.

Activate long-paper chunking using PS2 context-window strategy.

Configure tone and audience instructions.

### 📑 Module 2: Section Loop

Summarize each section (≤150 words).

Preserve original order.

Apply PS2 constraints.

Flag missing, empty, or short sections.

### 🛡️ Module 3: Guardrails

Handle missing or empty sections.

Flag sections under 50 words.

Prevent hallucinations.

Apply chunking safeguards.

### 🧩 Module 4: Rendering & Refinement

Assemble all required outputs.

Ensure consistent formatting.

Produce expert and lay summaries.

Create the glossary.

Generate the Checks & Warnings list.

### 📝 Module 5: Citation Extraction & Verification

Extract citations from the paper.

Verify MLA compliance.

Flag citation issues.

Add citation warnings to Checks & Warnings.

### 🎯 Module 6: Key Contributions & Rhetorical Moves Detector

Identify main contributions.

Detect rhetorical structures (problem statement, justification, conclusion).

Provide notes for expert/lay summaries.

Flag weak or missing rhetorical elements.

## 📎 Assignment Deliverables (Submitted Separately)

Screenshot of Copilot running the meta-prompt

Verification paragraph

Text-based flowchart

These are turned in via Canvas and not stored in the repository.
