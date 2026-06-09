# Nemotron 3 Ultra — who trains whom

An interactive explainer for the **post-training pipeline of NVIDIA Nemotron 3 Ultra** (550B-A55B, technical report 2026-06-04), built around **MOPD — Multi-teacher On-Policy Distillation**.

### ▶ Live page: https://butanium.github.io/nemotron-3-ultra-mopd/

It answers one question clearly: for every **student checkpoint** and every **teacher**, what model is it *initialized from*, what *data* and *algorithm* built it, and how does it feed the next student.

## What's in it

- **An interactive provenance graph.** Pan / zoom; **click any node** to highlight its connected arrows, dim the rest, and read an extended note on what its algorithm actually means (RLVR, PivotRL, SWE-RL, GenRM, MOPD…).
  - *dashed, coloured by source* = "initialized from" (weights forked) — the thick green dashed spine is the student backbone; thin green dashed lines are teachers forking off the student.
  - *solid green* = MOPD distillation (a teacher grading the student's rollouts, pulled in).
  - *dashed purple* = the Ultra GenRM's reward signal; *dotted grey* = an out-of-family model generating data.
- **A reverse-KL section** with a visual ↔ math toggle: an interactive plot of *forward (mass-covering) vs reverse (mode-seeking)* KL that shows why MOPD points the KL the way it does, plus the equations (objective, reverse-KL, the asynchronous clipped surrogate) rendered with MathJax.
- **The whole pipeline as a table** (Model · Init from · Data · Algorithm · Feeds →).

## Files

| file | what |
|---|---|
| `index.html` | the page (self-contained except MathJax via CDN) |
| `lineage.svg` | the graph, fetched + made interactive at runtime |
| `lineage.png` | static render of the same graph |
| `lineage.dot` | Graphviz source — regenerate with `dot -Tsvg lineage.dot -o lineage.svg` |

Because the graph is fetched at runtime, view it over HTTP (GitHub Pages, or `python3 -m http.server` in this folder), not via `file://`.

## Accuracy

Every node's *init / data / algorithm*, all the edges, and the equations are taken from the NVIDIA Nemotron 3 Ultra technical report, §3.3 + Figures 9–10 + Tables 4–5 (equations from §3.3.1). The MOPD structure, the iteration-2 teachers forking off Ultra MOPD1, the self-teacher and the reverse-KL objective are all explicit in the report.

A few **"initialized-from"** edges are inferred where the report is vague, and the page says so in its bottom accuracy note: the report gives no init checkpoint for the terminal-use / conversational-tool-use / model-usability / agentic-safety teachers and only "an Ultra checkpoint" for search — Figure 10 groups these on a dedicated **agentic SFT/RL path** (intro, p3: "agentic teachers built on a dedicated agentic SFT path"), drawn here forking from Ultra Base since the only documented anchor is the SWE teacher ("SFT to the Ultra base model on a blend of agentic data", p22). The chat teacher's init is likewise unnamed in prose and placed on the RLVR student per Figure 10. The animated probability curves in the reverse-KL visual are illustrative — the report does not publish token distributions.

Educational explainer; not affiliated with NVIDIA.
