# Module 2: Section Loop

> Change Log (2025-11-28)
> - Added `summary_level` variable to control per-section summary style.
> - Implemented conditional behavior for `"short"` vs `"detailed"` summaries.
> - Clarified that all summaries must still respect PS2 word limits and section order.

## Purpose

Iterate over each section of the paper in the original order and generate a summary that respects the PS2 constraints and the requested `summary_level`.

## Inputs

- Normalized list of sections from Module 1 (each with:
  - section title
  - section text
  - section index / order)
- `summary_level` parameter (string), one of:
  - `"short"`
  - `"detailed"`
- Global PS2 constraints (MLA, ≤150 words per section, total ≤1000 words).

## Core Logic

For each section in order:

1. **Check basic validity**
   - If section text is missing, empty, or consists only of whitespace:
     - Do not attempt a normal summary.
     - Record that the section is "missing/empty" for Module 3 (Guardrails) and final Checks & Warnings.
   - If section text is very short (< 50 words):
     - Still attempt a summary, but mark it as "very short" so that Guardrails can attach a warning.

2. **Apply `summary_level` behavior**

   - If `summary_level = "short"`:
     - Generate a compact, 1–2 sentence summary for the section.
     - Focus only on the most central idea, staying well under the 150-word limit.
     - Do **not** generate bullet points for this mode.
     - Ensure the summary is grounded strictly in the section text.

   - If `summary_level = "detailed"`:
     - Generate a short paragraph summary of the section (still ≤150 words total including bullets).
     - In addition, generate a bullet list of **3–5 key points** that:
       - Highlight main arguments, results, or contributions.
       - Are directly supported by the section text.
     - The paragraph + bullets together must not exceed the 150-word per-section constraint from PS2.
     - Maintain a formal academic tone.

3. **Enforce constraints**
   - Preserve the original section order from the paper.
   - Ensure the per-section word limit (≤150 words) is not exceeded in any mode.
   - Contribute to the global word limit (total summary ≤1000 words) enforced at a higher level.
   - Do not introduce ideas, citations, or results that are not explicitly present in the section text.

4. **Store results for later modules**
   - For each section, store:
     - Section title
     - Section index / position
     - Generated summary (and bullets, if `summary_level = "detailed"`)
     - Flags:
       - `is_missing_or_empty`
       - `is_very_short`
   - Pass this structured data to:
     - Module 3 (Guardrails) for warnings and hallucination checks.
     - Module 4 (Rendering & Refinement) for final formatting (table, expert/lay summaries, etc.).
