# LLM Book — Production Recipe Using Claude Projects
## Complete Self-Contained Reference

**What you start with:** one file — `llm_course_syllabus_v2.md`
**What you will produce:** 12 chapter draft files

**Notation in this document:**
- **[HUMAN]** = you do this manually
- **[CLAUDE]** = Claude does this; you type the prompt and copy the output

---

## PHASE 0 — ONE-TIME PROJECT SETUP

---

### Step 0.1 [HUMAN] — Create the Project

1. Go to claude.ai
2. Click **Projects** in the left sidebar
3. Click **New Project**
4. Name it: `LLM Book`

---

### Step 0.2 [HUMAN] — Set the Project Instructions

In the Project settings, paste the following as the Project Instructions (this is the persistent system prompt applied to every conversation in the project):

```
You are writing chapters of a graduate-level technical textbook on 
Large Language Models and Agentic Systems.

TARGET AUDIENCE
Software engineers and computer scientists with graduate-level knowledge 
of computer architecture and some prior exposure to basic neural networks. 
Mathematical maturity is assumed: linear algebra, multivariate calculus, 
and probability. No prior deep learning specialisation assumed.

MATHEMATICAL TREATMENT
Hybrid: key results stated precisely with full notation; the most important 
derivations carried out in full (the Agent Writing Brief for each chapter 
specifies which ones); secondary results stated with proofs sketched or 
referenced. No code. Worked conceptual examples used throughout.

MATHEMATICAL NOTATION — USE CONSISTENTLY IN ALL OUTPUT
- Inline math: $...$
- Display equations: $$...$$
- Bold uppercase for matrices: $\mathbf{W}$, $\mathbf{Q}$, $\mathbf{K}$, $\mathbf{V}$
- Bold lowercase for vectors: $\mathbf{x}$, $\mathbf{h}$, $\mathbf{w}$
- Italic for scalars: $n$, $d$, $L$, $\gamma$, $\eta$
- Greek letters for parameters: $\theta$, $\pi$, $\sigma$, $\alpha$, $\beta$

CITATION FORMAT
Author-year in body text, e.g. (Vaswani et al., 2017).
Full reference list at end of each chapter under ## References.
Do not invent, modify, or omit any citation specified in the syllabus.

OUTPUT FORMAT
All output is valid Markdown (.md) with LaTeX math delimiters as above.

MANDATORY AGENT RULES
1. Do not modify content between <!-- LOCKED --> and <!-- END LOCKED -->.
2. Do not remove or alter any HTML comment blocks (<!-- ... -->).
3. When you see <!-- PENDING: [instruction] -->, replace the block with 
   the requested content but preserve the comment tags until a human 
   removes them after review.
4. Do not reword, restructure, or improve any content you were not 
   explicitly instructed to change.

The complete 12-chapter syllabus, including summaries, primary references, 
further reading, and Agent Writing Briefs, is in the file 
llm_course_syllabus_v2.md in Project Knowledge. 
Read the relevant chapter entry at the start of each session.
```

---

### Step 0.3 [HUMAN] — Upload files to Project Knowledge

Click **Add content** (or equivalent) in the Project Knowledge section.
Upload the following two files:

**File 1:** `llm_course_syllabus_v2.md`
*(This is the file you already have. Upload it as-is. Never modify it.)*

**File 2:** Create a new text file on your computer called `register.md` 
with the following content, then upload it:

```markdown
# Chapter Register

This file is updated after each chapter is completed.
It summarises what each chapter covered, key terminology 
and notation choices, and forward references made.
Future chapter sessions should read this file to avoid 
repetition and maintain consistency.

## Status

No chapters completed yet.
```

---

## PHASE 1 — WRITING ONE CHAPTER

Repeat this phase for each of the 12 chapters, in order.

---

### Step 1.1 [HUMAN] — Open a new conversation in the project

- Go to the `LLM Book` project
- Click **New conversation** (or equivalent)
- Do NOT reuse a previous chapter's conversation
- One conversation per chapter

The Project Instructions and Project Knowledge 
(`llm_course_syllabus_v2.md` and `register.md`) 
load automatically into this conversation.

---

### Step 1.2 [HUMAN] — Find the chapter details in the syllabus

Before typing anything, open `llm_course_syllabus_v2.md` locally 
and find the entry for Lesson N. You need:
- The chapter title
- The "Cover in sequence" list inside the Agent Writing Brief section

Split the "Cover in sequence" list into three roughly equal groups.
These become the scope of Part 1, Part 2, and Part 3.

**Example for Chapter 2:**
The Agent Writing Brief lists 7 items. Split as:
- Part 1: items 1–2 (perceptron, MLP, activation functions, UAT)
- Part 2: items 3–5 (computational graph, backpropagation, Adam, initialisation)
- Part 3: items 6–7 (dynamical systems, normalisation) + references

---

### Step 1.3 [CLAUDE] — Generate Part 1

Type the following prompt. Fill in the bracketed fields from the syllabus.

```
I am starting Chapter [N]: [Chapter Title from syllabus].

Please read:
- llm_course_syllabus_v2.md in Project Knowledge, specifically 
  the Lesson [N] entry (Summary, Primary References, 
  Further Reading, and Agent Writing Brief)
- register.md in Project Knowledge, for context on what 
  previous chapters covered and which notation choices 
  have already been established

Then write PART 1 of Chapter [N].

PART 1 covers the following sections from the Agent Writing Brief:
[paste items 1 to X from the "Cover in sequence" list]

Target length: approximately 3,000–3,500 words.

Rules:
- Begin with the ## heading of the first section
- Do not write a chapter-level introduction paragraph before the 
  first section heading
- Do not write a conclusion
- End the output with this exact line: <!-- END PART 1 -->
- If a section listed above is only partially completed, 
  end it with: <!-- PENDING: [description of what remains] -->
```

**After Claude responds:**
Copy the full output into a new local file named `ch[N]_part1.md`.
Save it. Do not close the conversation.

---

### Step 1.4 [CLAUDE] — Generate Part 2

In the same conversation, type:

```
Here is Part 1 of Chapter [N], already written:

---
[paste the entire content of ch[N]_part1.md here]
---

Now write PART 2 of Chapter [N].

PART 2 covers the following sections from the Agent Writing Brief:
[paste items X+1 to Y from the "Cover in sequence" list]

Target length: approximately 3,000–3,500 words.

Rules:
- Maintain exactly the same notation and terminology established in Part 1
- Do not repeat or summarise content already covered in Part 1
- Begin directly with the ## heading of the next section
- End the output with: <!-- END PART 2 -->
```

**After Claude responds:**
Copy the full output into a new local file named `ch[N]_part2.md`. Save it.

---

### Step 1.5 [CLAUDE] — Generate Part 3

In the same conversation, type:

```
Here are Parts 1 and 2 of Chapter [N], already written:

---
[paste the entire content of ch[N]_part1.md here]
---
[paste the entire content of ch[N]_part2.md here]
---

Now write PART 3 of Chapter [N]. This is the final part.

PART 3 covers the following sections from the Agent Writing Brief:
[paste remaining items from the "Cover in sequence" list]

After the last content section, add a ## References section 
containing the full reference list. The references to include 
are listed under "Primary References" and "Further Reading" 
in the Lesson [N] entry of llm_course_syllabus_v2.md. 
Use the exact author, year, title, and venue as written there.

Target length: approximately 3,000–3,500 words for content, 
plus the reference list.

Rules:
- Maintain exactly the same notation and terminology as Parts 1 and 2
- Do not repeat content already covered
- End the output with: <!-- END PART 3 — CHAPTER [N] COMPLETE -->
```

**After Claude responds:**
Copy the full output into a new local file named `ch[N]_part3.md`. Save it.

---

### Step 1.6 [HUMAN] — Assemble the chapter

On your computer, create a new file named `ch[N]_draft.md`.
Paste into it, in order:
1. Contents of `ch[N]_part1.md`
2. One blank line
3. Contents of `ch[N]_part2.md`
4. One blank line
5. Contents of `ch[N]_part3.md`

Save `ch[N]_draft.md`. This is your chapter draft.

---

### Step 1.7 [CLAUDE] — Generate the register entry

In the same conversation, type:

```
The chapter is now complete. Produce a register entry to be 
appended to register.md.

The entry should be 150–200 words and cover:
1. Chapter number and title
2. Completion status (complete / sections pending)
3. Key terminology choices (any term whose phrasing could 
   vary and should be used consistently in future chapters)
4. Key notation choices (any variable, symbol, or formula 
   introduced for the first time in this chapter)
5. Forward references placed (which future chapters were 
   cited, and what was promised to them)
6. Any deviations from the Agent Writing Brief

Format it as a Markdown section starting with:
## Chapter [N]: [Title]

Here is the complete chapter draft for you to analyse:

[paste the full content of ch[N]_draft.md here]
```

---

### Step 1.8 [HUMAN] — Update register.md

1. Copy the register entry Claude produced
2. Open your local `register.md`
3. Append the entry at the bottom of the file
4. Save `register.md`
5. Go to Project Knowledge in the `LLM Book` project
6. Delete the current `register.md`
7. Upload the updated `register.md`

---

### Step 1.9 [HUMAN] — Upload the chapter draft to Project Knowledge
*(Optional but recommended up to Chapter 9; see note below)*

Upload `ch[N]_draft.md` to Project Knowledge.
This allows future chapter sessions to see what was written 
and avoid repetition or contradictions.

> **Context management note for Chapters 9–12:**
> Each file in Project Knowledge consumes space in the 
> context window. By Chapter 10, you may have 9 chapter 
> drafts uploaded, which can crowd the context.
> If responses start feeling truncated or the model seems 
> to lose track of instructions, remove the oldest chapter 
> drafts from Project Knowledge, keeping only:
> - `llm_course_syllabus_v2.md` (always keep)
> - `register.md` (always keep — it summarises all chapters)
> - The 2 most recently completed chapter drafts

---

## PHASE 2 — REVISING A CHAPTER

Use this phase after reading a chapter draft yourself or after 
getting feedback from another AI (e.g. Gemini, GPT-4).

---

### Step 2.1 [HUMAN or CLAUDE] — Extract specific revision items

If you have proofreader feedback (from another AI or yourself), 
open a new conversation in the project and type:

```
The following is proofreader feedback on a section of Chapter [N].

Extract a numbered revision list. For each item include:
  (a) Location: the paragraph, equation, or sentence where the 
      issue appears (quote the first few words of the sentence 
      if needed to identify it)
  (b) Issue: the specific problem
  (c) Change: the specific correction required

Exclude:
- General positive comments
- Vague stylistic suggestions with no specific target passage
- Any suggestion that would add code to the text
- Any suggestion that contradicts hybrid mathematical treatment

PROOFREADER FEEDBACK:
[paste the feedback here]
```

Review the resulting list. Remove any item you disagree with.

---

### Step 2.2 [CLAUDE] — Apply targeted revisions

In the same conversation, type:

```
You are revising a specific section of Chapter [N]: [Title].

MANDATORY RULES:
- Make ONLY the changes listed below
- Preserve all other content exactly, word for word
- Do not restructure, reword, or improve anything not listed
- Do not remove any <!-- comment --> blocks
- Return the complete revised section text

CURRENT SECTION TEXT:
---
[paste only the specific section being revised, 
 not the whole chapter]
---

REVISION LIST:
1. Location: [paragraph / equation / sentence starting with "..."]
   Issue: [what is wrong]
   Change: [what to do]

2. Location: ...
   Issue: ...
   Change: ...
```

---

### Step 2.3 [HUMAN] — Update the chapter file

1. Copy the revised section from Claude's response
2. Open `ch[N]_draft.md`
3. Find the corresponding section
4. Replace it with the revised version
5. Save `ch[N]_draft.md`
6. If the chapter is in Project Knowledge: delete the old version, upload the updated one

---

## QUICK REFERENCE — FILE NAMES

| File | Created by | Lives in | Updated |
|------|-----------|---------|---------|
| `llm_course_syllabus_v2.md` | Already exists | Project Knowledge | Never |
| `register.md` | You (Step 0.3) | Project Knowledge | After each chapter (Step 1.8) |
| `ch[N]_part1.md` | Claude (Step 1.3) | Local only | Never |
| `ch[N]_part2.md` | Claude (Step 1.4) | Local only | Never |
| `ch[N]_part3.md` | Claude (Step 1.5) | Local only | Never |
| `ch[N]_draft.md` | You (Step 1.6) | Local + Project Knowledge | After revisions (Step 2.3) |

---

## QUICK REFERENCE — WHICH STEPS ARE HUMAN VS CLAUDE

| Step | Type | What happens |
|------|------|-------------|
| 0.1 Create project | **HUMAN** | Click through claude.ai UI |
| 0.2 Set instructions | **HUMAN** | Paste the Project Instructions text |
| 0.3 Upload files | **HUMAN** | Upload syllabus + register |
| 1.1 Open conversation | **HUMAN** | Click new conversation in project |
| 1.2 Find chapter details | **HUMAN** | Read syllabus, split sections into 3 parts |
| 1.3 Generate Part 1 | **CLAUDE** | Paste prompt, copy output to file |
| 1.4 Generate Part 2 | **CLAUDE** | Paste prompt + Part 1, copy output |
| 1.5 Generate Part 3 | **CLAUDE** | Paste prompt + Parts 1–2, copy output |
| 1.6 Assemble chapter | **HUMAN** | Concatenate 3 part files into draft |
| 1.7 Generate register entry | **CLAUDE** | Paste prompt + draft, copy output |
| 1.8 Update register | **HUMAN** | Edit file, re-upload to Project Knowledge |
| 1.9 Upload chapter | **HUMAN** | Upload draft to Project Knowledge |
| 2.1 Extract revisions | **CLAUDE** | Paste feedback, copy revision list |
| 2.2 Apply revisions | **CLAUDE** | Paste section + list, copy revised text |
| 2.3 Update chapter file | **HUMAN** | Replace section in draft, re-upload |
