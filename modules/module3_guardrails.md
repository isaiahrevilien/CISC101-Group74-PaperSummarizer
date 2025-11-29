# Module 3: Guardrails

> Change Log (2025-11-28)
> - Added `evidence_mode` flag with `"strict"` behavior for evidence-based summarization.
> - Introduced standardized warning messages for missing/empty and very short sections.
> - Clarified hallucination prevention and how warnings surface in final output.

## Purpose

Enforce safety, evidence, and quality constraints on the summaries produced by Module 2, and ensure the system obeys the “only use what’s in the paper” rule. Provide standardized warnings that can be surfaced in the final Checks & Warnings section.

## Inputs

- Per-section summary data from Module 2, including:
  - Section title
  - Section index / position
  - Generated summary text (if any)
  - Flags:
    - `is_missing_or_empty`
    - `is_very_short`
- Global configuration flags:
  - `evidence_mode` (string), e.g.:
    - `"strict"`
    - `"default"` (or other non-strict modes)
- PS2 constraints and MLA expectations.

## Strict Evidence Mode

When `evidence_mode = "strict"`:

1. **Source-bound behavior**
   - Summaries must only include claims, equations, definitions, and results that:
     - Appear explicitly in the provided section text, **or**
     - Are straightforward restatements/rephrasings of that text.
   - No external knowledge, background facts, or invented details may be added.

2. **Insufficient evidence handling**
   - If the section text does **not** provide enough information to produce a meaningful summary under strict evidence rules:
     - The system must output an explicit notice in place of or in addition to the summary, for example:
       - “The source text does not provide enough detail to summarize this section in strict evidence mode.”
   - This notice should be attached to the section’s entry and surfaced in:
     - The Section-by-Section Table.
     - The Checks & Warnings list.

3. **Hallucination mitigation**
   - Guard against:
     - Claims not supported by the section text.
     - Citations, equations, or numerical values that do not appear in the input.
   - If such unsupported content is detected or likely:
     - Strip it from the summary where possible.
     - If it cannot be safely removed, replace the summary with a strict-evidence warning message.

## Section Warning Messages

For sections that are:

- Missing or empty (`is_missing_or_empty = true`), or
- Very short (`is_very_short = true`, e.g., < 50 words),

the module must produce standardized warning messages to be attached to that section and surfaced in the final output.

### Standard Warning Templates

Suggested standardized warnings:

- For missing/empty sections:
  - “Section skipped: no usable text was provided.”
- For very short sections (< 50 words):
  - “Section very short: summary may be incomplete or low confidence.”

These warnings should be:

- Stored alongside each section’s summary data.
- Explicitly passed to Module 4 so they appear in:
  - The Checks & Warnings section.
  - Optionally as notes in the Section-by-Section Table.

## General Guardrail Responsibilities

- Ensure that all summaries:
  - Respect PS2 constraints on word limits and ordering.
  - Do not introduce content not grounded in the paper.
- Aggregate all per-section issues into a structured list for final rendering, including:
  - Missing/empty sections.
  - Very short sections.
  - Strict-evidence insufficiency notices.
  - Any hallucination-related corrections.
- Provide clear, human-readable messages so users understand when and why a section’s summary may be limited or skipped.
