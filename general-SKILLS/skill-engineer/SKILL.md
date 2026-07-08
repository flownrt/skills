---
name: skill-engineer
description: Guidance for writing, editing, and debugging effective agent skills so they trigger reliably and perform once loaded. Use when the user says "create a skill", "new skill", "write a SKILL.md", "update, improve, or refactor a skill", "review my skill", or "why isn't my skill triggering", or describes a recurring workflow that should become a skill without naming "skill". Do NOT use to improve a plain prompt or system prompt (that is prompt-improvement), to design a multi-agent workflow or orchestration (that is workflow-architecture), or to turn a vague idea into a spec (that is scoping).
---

# Skill-Engineer

A skill is a folder the agent loads on demand: a `SKILL.md` (YAML frontmatter + markdown) at its root, plus optional `scripts/`, `references/`, and `assets/`. Skills follow a portable folder convention, so one built well runs unchanged on any agent that supports it. The job here is a deployable `SKILL.md` – a description that makes it fire at the right moment, and a lean body that makes it perform once it does – whenever a skill is written, edited, debugged, or reviewed, not only when one is created from scratch.

A skill loads in three stages: at **discovery** only `name` and `description` sit in context → dozens of skills cost almost nothing, and the description alone decides whether the skill fires; at **activation** the agent reads the full body once a request matches; at **execution** it opens a reference or runs a script only when it gets there → files cost nothing until opened. So two jobs fail for different reasons – the **description routes**, the **body executes** – and one rule sits above the rest: add only what the model does not already know.

---

## Before you write (the gate, always)

Three decisions, in order, before the first line.

1. **Which job is this?** **Create** (nothing yet) → run the whole arc below. **Edit / optimize** (it exists) → read the current `SKILL.md` first and change only what is failing; after a model upgrade, cut any classification or routing layer a stronger model now handles unaided. **Debug** (won't fire, or fires wrongly) → almost always the description, not the body; jump to step 1. **Review** (someone else's skill) → score it against the ship checklist and audit its scripts.

2. **Is it a skill, and which kind?** A task the agent repeatedly fumbles → skill candidate; a one-off → just do it; a change of tone or formatting only → user preferences or a system prompt, never a skill. Then pick the kind: a **capability primitive** wraps a deterministic tool (logic in code, the body only teaches invocation) for when *the agent can't do X* → mostly annotated commands; a **process primitive** encodes a method in prose for when *its process or output quality is poor*. Mature setups run both. Scale strictness to what a wrong move costs → many valid paths, loose heuristics; a preferred shape, a template; fragile or consistency-critical work such as migrations or document patching, an exact script and strict steps.

3. **Clarify what's decision-critical – once, batched, with defaults.** The recurring gap the skill removes, the real phrases that should trigger it, and whether it wraps a tool or encodes a method – or, for an edit or debug job, what exactly is failing now. Propose a default for each and proceed on it unless the answer would change the shape of the skill. Ask the batch in a single round, never one question at a time.

---

## How to run it

1. **Get the description right – it is the routing contract.** It is the only part read before the skill loads, so a skill that misfires has a wrong description far more often than a wrong body. Give it three things: *what* it does, *when* to fire it (the phrases users actually type, plus the implicit cases – "Use even if …" catches the ones that never say the word "skill"), and the *differentiator* from its neighbours so routing does not collide. Never fold the step-by-step into it → the agent follows that summary and skips the body; keep it neutral → `CRITICAL`/`MUST`/`ALWAYS` make capable models over-fire.
   - Weak: *"A helpful skill for documents."*
   - Strong: *"Fill PDF form fields, extract form data, flatten completed PDFs. Use when the user mentions PDF forms or field population. Do NOT use for scanned-image OCR."*

2. **Write the smallest body that works.** It carries the skeleton, the gates, and the templates; the model does the rest at runtime. Determinism belongs in code, judgement in prose → push anything fragile, repetitive, or where variation is a bug into a script. Show, don't describe – a commented command beats a paragraph about it, because the agent pattern-matches on syntax. Don't re-teach what the model knows, don't paste library source (install it), don't leave a load-bearing step to inference ("then deploy it" → "run `npm run deploy:staging`, wait for HTTP 200 from `/healthz`, then report success"), and never hard-code absolute paths (relative, forward slashes, placeholders). Then challenge every section: if it does not pin down something the model would otherwise get wrong, cut it.

3. **Wire in what makes bodies reliable.** A **validation loop** is the biggest single lever on quality → state verify → fix → re-verify (a visual QA pass before delivery; tests green before "done"; a schema check before emitting data), and for each step that can fail, say what failure looks like and what to do. **State-check before acting** → confirm setup, branch, then proceed. Load detail **just in time, one level deep** → point to a reference exactly when it is needed, link it straight from `SKILL.md` (never chain `a.md → b.md → c.md`, since a nested file may be previewed only partway), and give any reference over ~100 lines a table of contents. Show a **sample of any structured output**, and defer the long tail of options to `tool --help`.

4. **One concern, composable.** One skill is one capability or one discipline → design + planning + build + test in a single file is a framework, not a skill; split it. Where one skill's output feeds another, document that shape and let a shared repo file (`CONTEXT.md`, `AGENTS.md`) coordinate them instead of implicit handoffs. Skills can write persistent artifacts – a decision log, an ADR – that later sessions read back, the practical cure for agents having no memory. If it encodes a named discipline (TDD, red-green-refactor), say so, to give the agent a model to align to.

5. **Test both jobs – separately, on the weakest model you will ship on** (strong models forgive vague skills and hide the defects). Triggering → ask for something it should handle without naming it; if it does not fire, fix the description. Execution → invoke it explicitly; if the output is wrong, fix the body. Ask "which skill did you use?" to diagnose routing, have a second model attack it ("what edge cases break this?"), and keep a small eval set of prompts that *should* and *should not* fire. Skills snapshot at session start → restart after edits, or they won't take effect.

---

## Auditing a skill you didn't write

A skill runs arbitrary code and steers the agent, so an untrusted one is a data-exfiltration vector. Read every file; audit `scripts/` for outbound network calls, file access outside the expected scope, and command execution; scan references for injected instructions ("ignore previous instructions …"); check the name is not typosquatting a popular skill; run it sandboxed first; and pin to a specific version or commit, not "latest".

---

## Ship checklist

- `name` is lowercase-hyphens, 1–64 characters, and matches the folder exactly; frontmatter is valid YAML with no `<` or `>` (invalid YAML silently fails to load).
- The description gives what + when + differentiator, carries the real trigger phrases, and does not summarise the workflow.
- No human-facing docs in the folder (no `README`, `CHANGELOG`, install guide), no time-sensitive facts ("as of Q4 …" rots – fetch live or omit), relative paths only.
- A validation loop is documented, state-checks guard setup where relevant, and any script's output shape is shown.
- Tested for correct triggering *and* correct execution, on both a weak and a strong model.
- It does one thing, composes cleanly with its neighbours, and is under version control.
