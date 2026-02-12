---
---
name: generating-mcq-exams
description: |
  Generate comprehensive, tough, mixed-strategy MCQ exams from any source material.
  This skill should be used when users ask to create an exam, generate MCQs, make a quiz,
  build test questions, create assessment questions, or generate practice tests from a file,
  chapter, document, or any study material.
---

# MCQ Exam Generator

Generate mixed-strategy multiple-choice exams that use the right question type for each concept — from direct recall to complex scenarios.

## What This Skill Does
- Reads any source material (markdown, text, PDF, or other documents)
- Identifies every key concept, fact, framework, relationship, and mental model
- Generates a **mixed-strategy exam** using four question types at target proportions
- Produces a single markdown exam file with inline answers and explanations

## Question Type Mix

| Type | Target % | Bloom's Level | When to Use | Example Pattern |
|------|----------|---------------|-------------|-----------------|
| **Direct** | ~20% | 1-2 (Remember/Understand) | Definitions, terminology, key facts, numeric data, named entities | "What does X refer to?" / "Which of the following is a characteristic of Y?" |
| **Conceptual** | ~25% | 2-3 (Understand/Apply) | Distinctions, comparisons, cause-effect, "why" questions | "Why does X differ from Y?" / "What is the primary purpose of Z?" |
| **Applied Scenario** | ~40% | 3-5 (Apply/Analyze/Evaluate) | Frameworks in new contexts, role-based decisions, anti-pattern ID | "A [role] faces [situation]. Based on [framework], what should they do?" |
| **Evaluative Scenario** | ~15% | 5-6 (Evaluate/Create) | Counter-arguments, evidence evaluation, synthesis across concepts | "[Claim]. A critic argues [counter]. What is the best response?" |

### Mix Rationale
- **Direct questions** anchor foundational knowledge — you can't apply what you can't recall
- **Conceptual questions** verify understanding without scenario overhead
- **Applied scenarios** test transfer to new contexts (the exam's backbone)
- **Evaluative scenarios** separate mastery from competence
- **Cognitive pacing:** alternating short direct questions with longer scenarios prevents fatigue

## What This Skill Does NOT Do
- Create fill-in-the-blank, essay, or short-answer questions
- Grade or evaluate student responses
- Generate questions beyond the source material's content

---

## Before Implementation

| Source | Gather |
|--------|--------|
| **Source File** | Read the full file at the path provided by the user |
| **Conversation** | Any user preferences on focus areas, difficulty, or output path |

---

## Workflow

### Step 1: Analyze Source Material

Read the source file completely. Extract and categorize:

| Extract | Purpose |
|---------|---------|
| Key concepts and definitions | Foundation questions |
| Frameworks and models | Application questions |
| Comparisons and contrasts | Distinction questions |
| Processes and sequences | Order/phase questions |
| Statistics and specific data | Precision questions |
| Cause-effect relationships | Reasoning questions |
| Anti-patterns and pitfalls | Judgment questions |
| Real-world examples and case studies | Scenario questions |
| Named tools, people, organizations | Ecosystem questions |

### Step 2: Plan Sections

Group related concepts into logical exam sections. Each section gets a `### Section X: Title` header. Aim for 3-6 questions per section — more for dense topics, fewer for lighter ones.

### Step 3: Plan Answer Distribution

Before writing questions, pre-assign correct answer positions to ensure roughly equal A/B/C/D distribution (~25% each). Use this approach:

1. Decide total question count
2. Divide into quartiles and assign letters cyclically with variation
3. Verify: no more than 3 consecutive same-letter answers
4. Verify: each letter appears 23-27% of total

See `references/answer-distribution.md` for the distribution algorithm.

### Step 4: Write Questions

For each question, follow ALL rules from `references/question-design.md` and `references/option-design.md`.

**Choose the right question type for each concept:**

- **Direct** (~20%) — Use for definitions, terminology, key facts, specific numbers, named entities.
- **Conceptual** (~25%) — Use for comparisons, distinctions, cause-effect, purpose/rationale.
- **Applied Scenario** (~40%) — Use for frameworks, models, processes, anti-patterns. Present a situation, ask for analysis.
- **Evaluative Scenario** (~15%) — Use for counter-arguments, evidence evaluation, synthesis.

**All question types must be:**
- Self-contained (never say "the chapter," "the text," "the reading," "according to the passage")
- Embed necessary context directly in the question stem

**CRITICAL: Difficulty and Option Length Standards (applies to ALL question types, including Direct and Conceptual)**

The exam must be TOUGH. This means:

1. **ALL options must be 20-40 words.** Even for Direct and Conceptual questions, every option must contain substantive reasoning, a plausible mechanism, or a detailed explanation. Short 5-15 word options make distractors obviously wrong. When every option is 25-35 words with real technical reasoning, students must actually understand the material to distinguish correct from incorrect.

2. **ALL distractors must sound like they could be correct.** Each wrong option must use real terminology from the source material, reference real concepts, and present plausible-sounding mechanisms. A student who skimmed the material should find at least 2 options compelling.

3. **Question stems must embed scenarios even for recall questions.** Instead of "What is X?", write "A developer encounters [situation]. [Context]. What explains this?" — this forces application even for factual knowledge.

4. **Every option must have comparable depth.** If the correct answer explains a mechanism with cause and effect, EVERY distractor must also explain a mechanism with cause and effect. If the correct answer names a concept and explains why, EVERY distractor must name a concept and explain why.

**Example of WEAK options (too easy — short distractors give away the answer):**
```
- A) To provide persistent project context that Claude Code loads at session start
- B) To store API keys securely
- C) To serve as a README replacement
- D) To cache conversation histories
```

**Example of TOUGH options (all substantive, all plausible, all ~30 words):**
```
- A) To provide persistent project context that Claude Code automatically loads at the start of every session, giving Claude immediate understanding of the project without repeated explanations
- B) To store Claude Code's internal configuration settings and API credentials in a human-readable format that persists across sessions and can be version-controlled alongside the project codebase
- C) To serve as a structured README replacement that documents the project for both human developers and AI agents, following a standardized format that enables automated parsing and navigation
- D) To cache previous conversation histories in markdown format so Claude can resume interrupted sessions by re-reading the cached dialogue and restoring the previous conversation state automatically
```

**The second example is harder because every option sounds plausible, uses real terminology, and has similar length and specificity. This is the standard for EVERY question.**

- Every wrong option must be plausible at the question's complexity level
- Correct option NOT identifiable by length, specificity, or hedging language

### Step 4.5: MANDATORY Per-Question Word Count Audit (DO NOT SKIP)

**After writing EACH question's four options, you MUST perform this audit before moving to the next question.** This is the most critical quality gate in the entire exam generation process.

**Procedure — for every single question:**

1. **Count the words** in each option (A, B, C, D). Count precisely — do not estimate.
2. **Calculate the average** word count across all four options.
3. **Check each option** — does any option deviate more than 20% from the average?
   - If the average is 25 words, every option must be 20–30 words.
   - If the average is 12 words, every option must be 10–15 words.
4. **Check the correct answer specifically** — is it the longest option? If YES, you MUST fix it before proceeding:
   - **Preferred fix**: Compress the correct answer by removing subordinate clauses, qualifiers, or restating more concisely
   - **Alternative fix**: Expand ALL distractors with plausible reasoning, consequences, or mechanisms (see `references/option-design.md` for techniques)
   - **Never** pad distractors with filler — added length must be substantive
5. **Check for specificity imbalance** — does the correct answer contain more concrete details, qualifiers ("may," "can," "often"), or technical terms than the distractors? If YES, equalize specificity across all options.

**The "Write Distractors First" technique (STRONGLY RECOMMENDED):**

For each question, write the three WRONG options FIRST at your natural length. Then write the correct answer constrained to match their average word count. This prevents the natural tendency to over-explain the correct answer.

**Example of a FAILING question and its fix:**

```
FAILING (correct = A, 31 words; B = 14 words; C = 12 words; D = 15 words):
- A) Claude Code re-sends the entire conversation history with each new message, creating the illusion of continuity while the underlying LLM remains completely stateless between calls ← OBVIOUS by length
- B) The LLM retains a small internal cache for the session
- C) Claude Code compresses messages into embeddings for the LLM
- D) Conversation state is stored in a cloud database between calls

FIXED (A = 18, B = 17, C = 16, D = 17 words):
- A) Claude Code re-sends the full conversation history with each message, so the LLM reads everything fresh
- B) The LLM retains a small internal session cache that persists between calls and stores recent interactions
- C) Claude Code compresses previous messages into vector embeddings that the LLM decodes on demand
- D) Conversation state is stored in a cloud database and retrieved by Claude Code for each request
```

**This step is non-negotiable.** A test-savvy student should NEVER be able to identify the correct answer by length alone. If you skip this step, the exam fails its primary quality requirement.

### Step 5: Write Inline Answers

Place the answer and explanation in a blockquote directly after each question's options:

```markdown
> **Answer: X** — Explanation text referencing the specific concept and why other options fail.
```

### Step 6: Assemble Output File

Write to `<source_name>_exam.md` in the same directory as the source file (or user-specified path).

Structure:
```
# Exam Title
Instructions paragraph
---
### Section A: Topic
**Q1.** Question text
- A) Option
- B) Option
- C) Option
- D) Option
> **Answer: X** — Explanation
---
**Q2.** ...
```

### Step 7: Validate

Run through the output checklist below before delivering.

---

## Output Checklist

### Question Quality
- [ ] Question type mix is approximately: ~20% Direct, ~25% Conceptual, ~40% Applied Scenario, ~15% Evaluative Scenario
- [ ] **NO type tags in the exam output** — do NOT prefix questions with `[Direct]`, `[Conceptual]`, `[Applied]`, or `[Evaluative]`. The question type is an internal planning tool only; the student-facing exam must be clean
- [ ] Direct and Conceptual questions are concise — stems are 1-3 sentences, options are 5-15 words
- [ ] Applied and Evaluative questions use scenario stems with embedded context
- [ ] Every question is self-contained (no "the chapter says," "according to the text," "the reading mentions")
- [ ] Sufficient context is embedded in each question stem for standalone comprehension
- [ ] All major concepts from the source material are covered
- [ ] Question types vary within sections — no 5+ consecutive scenario questions or 5+ consecutive direct questions

### Option Quality (HIGHEST PRIORITY — check this FIRST)
- [ ] **ZERO TOLERANCE: For EVERY question, count the words in each option. The correct answer must NOT be the longest option in ANY question.** If even ONE question has the correct answer as the longest option, the entire exam fails this check. Go back and fix it by compressing the correct answer or expanding the distractors with substantive reasoning.
- [ ] **Per-question word count balance**: For each question, all four options must be within ~20% word count of each other. Calculate: if the average is N words, every option must be between 0.8N and 1.2N words. No exceptions.
- [ ] **Audit trail**: After the exam, include an internal word-count spot-check of 5 randomly selected questions showing (A: Xw, B: Xw, C: Xw, D: Xw, correct: [letter], longest: [letter]). The correct answer must NOT be the longest in any of them.
- [ ] Direct/Conceptual questions have short, crisp options (5-15 words) — not artificially padded
- [ ] Scenario questions have naturally longer options — but still balanced within each question
- [ ] Every distractor is plausible — sounds like something a partially-informed student would believe
- [ ] Correct answer is NOT consistently the longest, most hedged, or most specific option
- [ ] No "all of the above" or "none of the above" options
- [ ] **Specificity balance**: The correct answer does not contain noticeably more technical terms, concrete details, or qualifying language than the distractors

### Answer Distribution
- [ ] Each of A, B, C, D appears as the correct answer roughly 23-27% of the time
- [ ] No more than 3 consecutive questions share the same correct answer letter
- [ ] Distribution verified by counting after all questions are written

### Format
- [ ] File is valid markdown
- [ ] Title and instructions at top
- [ ] Sections with `### Section` headers
- [ ] Questions bold-numbered (`**Q1.**`)
- [ ] Options as `- A)` through `- D)` bullet list
- [ ] Answer in blockquote directly after each question: `> **Answer: X** — explanation`
- [ ] Horizontal rules (`---`) between questions
- [ ] No separate answer key at the bottom (answers are inline only)

---

## Reference Files

| File | When to Read |
|------|--------------|
| `references/question-design.md` | Before writing any questions — contains question stem rules and scenario patterns |
| `references/option-design.md` | Before writing any options — contains distractor strategies and length-balancing rules |
| `references/answer-distribution.md` | During Step 3 — contains the distribution algorithm and verification method |

---
# MCQ Exam Generator

Generate mixed-strategy multiple-choice exams that use the right question type for each concept — from direct recall to complex scenarios.

## What This Skill Does

- Reads any source material (markdown, text, PDF, or other documents)
- Identifies every key concept, fact, framework, relationship, and mental model
- Generates a **mixed-strategy exam** using four question types at target proportions
- Produces a single markdown exam file with inline answers and explanations

## Question Type Mix

| Type                          | Target % | Bloom's Level                | When to Use                                                       | Example Pattern                                                              |
| ----------------------------- | -------- | ---------------------------- | ----------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **Direct**              | ~20%     | 1-2 (Remember/Understand)    | Definitions, terminology, key facts, numeric data, named entities | "What does X refer to?" / "Which of the following is a characteristic of Y?" |
| **Conceptual**          | ~25%     | 2-3 (Understand/Apply)       | Distinctions, comparisons, cause-effect, "why" questions          | "Why does X differ from Y?" / "What is the primary purpose of Z?"            |
| **Applied Scenario**    | ~40%     | 3-5 (Apply/Analyze/Evaluate) | Frameworks in new contexts, role-based decisions, anti-pattern ID | "A [role] faces [situation]. Based on [framework], what should they do?"     |
| **Evaluative Scenario** | ~15%     | 5-6 (Evaluate/Create)        | Counter-arguments, evidence evaluation, synthesis across concepts | "[Claim]. A critic argues [counter]. What is the best response?"             |

### Mix Rationale

- **Direct questions** anchor foundational knowledge — you can't apply what you can't recall
- **Conceptual questions** verify understanding without scenario overhead
- **Applied scenarios** test transfer to new contexts (the exam's backbone)
- **Evaluative scenarios** separate mastery from competence
- **Cognitive pacing:** alternating short direct questions with longer scenarios prevents fatigue

## What This Skill Does NOT Do

- Create fill-in-the-blank, essay, or short-answer questions
- Grade or evaluate student responses
- Generate questions beyond the source material's content

---

## Before Implementation

| Source                 | Gather                                                          |
| ---------------------- | --------------------------------------------------------------- |
| **Source File**  | Read the full file at the path provided by the user             |
| **Conversation** | Any user preferences on focus areas, difficulty, or output path |

---

## Workflow

### Step 1: Analyze Source Material

Read the source file completely. Extract and categorize:

| Extract                              | Purpose               |
| ------------------------------------ | --------------------- |
| Key concepts and definitions         | Foundation questions  |
| Frameworks and models                | Application questions |
| Comparisons and contrasts            | Distinction questions |
| Processes and sequences              | Order/phase questions |
| Statistics and specific data         | Precision questions   |
| Cause-effect relationships           | Reasoning questions   |
| Anti-patterns and pitfalls           | Judgment questions    |
| Real-world examples and case studies | Scenario questions    |
| Named tools, people, organizations   | Ecosystem questions   |

### Step 2: Plan Sections

Group related concepts into logical exam sections. Each section gets a `### Section X: Title` header. Aim for 3-6 questions per section — more for dense topics, fewer for lighter ones.

### Step 3: Plan Answer Distribution

Before writing questions, pre-assign correct answer positions to ensure roughly equal A/B/C/D distribution (~25% each). Use this approach:

1. Decide total question count
2. Divide into quartiles and assign letters cyclically with variation
3. Verify: no more than 3 consecutive same-letter answers
4. Verify: each letter appears 23-27% of total

See `references/answer-distribution.md` for the distribution algorithm.

### Step 4: Write Questions

For each question, follow ALL rules from `references/question-design.md` and `references/option-design.md`.

**Choose the right question type for each concept:**

- **Direct** (~20%) — Use for definitions, terminology, key facts, specific numbers, named entities. Keep stems short (1-2 sentences). Options should be concise and distinct.
- **Conceptual** (~25%) — Use for comparisons, distinctions, cause-effect, purpose/rationale. Stems are 1-3 sentences. Options explain reasoning briefly.
- **Applied Scenario** (~40%) — Use for frameworks, models, processes, anti-patterns. Present a situation, ask for analysis. Stems are 2-4 sentences with embedded context.
- **Evaluative Scenario** (~15%) — Use for counter-arguments, evidence evaluation, synthesis. Stems are 3-5 sentences with nuanced setups.

**All question types must be:**

- Self-contained (never say "the chapter," "the text," "the reading," "according to the passage")
- Embed necessary context directly in the question stem

**Option length should match question type:**

- Direct/Conceptual questions: options can be short (5-15 words) — don't pad them artificially
- Applied/Evaluative scenarios: options are naturally longer (15-35 words) — maintain ~20% balance within each question
- Every wrong option must be plausible at the question's complexity level
- Correct option NOT identifiable by length, specificity, or hedging language

### Step 5: Write Inline Answers

Place the answer and explanation in a blockquote directly after each question's options:

```markdown
> **Answer: X** — Explanation text referencing the specific concept and why other options fail.
```

### Step 6: Assemble Output File

Write to `<source_name>_exam.md` in the same directory as the source file (or user-specified path).

Structure:

```
# Exam Title
Instructions paragraph
---
### Section A: Topic
**Q1.** Question text
- A) Option
- B) Option
- C) Option
- D) Option
> **Answer: X** — Explanation
---
**Q2.** ...
```

### Step 7: Validate

Run through the output checklist below before delivering.

---

## Output Checklist

### Question Quality

- [ ] Question type mix is approximately: ~20% Direct, ~25% Conceptual, ~40% Applied Scenario, ~15% Evaluative Scenario
- [ ] **NO type tags in the exam output** — do NOT prefix questions with `[Direct]`, `[Conceptual]`, `[Applied]`, or `[Evaluative]`. The question type is an internal planning tool only; the student-facing exam must be clean
- [ ] Direct and Conceptual questions are concise — stems are 1-3 sentences, options are 5-15 words
- [ ] Applied and Evaluative questions use scenario stems with embedded context
- [ ] Every question is self-contained (no "the chapter says," "according to the text," "the reading mentions")
- [ ] Sufficient context is embedded in each question stem for standalone comprehension
- [ ] All major concepts from the source material are covered
- [ ] Question types vary within sections — no 5+ consecutive scenario questions or 5+ consecutive direct questions

### Option Quality

- [ ] **CRITICAL: The correct answer must NOT be longer than the distractors.** After writing each question, count the words in each option. If the correct answer is the longest, either compress it or expand the distractors to match. A test-savvy student should never be able to pick the right answer by length alone
- [ ] All four options per question have comparable length (within ~20% of each other)
- [ ] Direct/Conceptual questions have short, crisp options (5-15 words) — not artificially padded
- [ ] Scenario questions have naturally longer options — but still balanced within each question
- [ ] Every distractor is plausible — sounds like something a partially-informed student would believe
- [ ] Correct answer is NOT consistently the longest, most hedged, or most specific option
- [ ] No "all of the above" or "none of the above" options

### Answer Distribution

- [ ] Each of A, B, C, D appears as the correct answer roughly 23-27% of the time
- [ ] No more than 3 consecutive questions share the same correct answer letter
- [ ] Distribution verified by counting after all questions are written

### Format

- [ ] File is valid markdown
- [ ] Title and instructions at top
- [ ] Sections with `### Section` headers
- [ ] Questions bold-numbered (`**Q1.**`)
- [ ] Options as `- A)` through `- D)` bullet list
- [ ] Answer in blockquote directly after each question: `> **Answer: X** — explanation`
- [ ] Horizontal rules (`---`) between questions
- [ ] No separate answer key at the bottom (answers are inline only)

---

## Reference Files

| File                                  | When to Read                                                                            |
| ------------------------------------- | --------------------------------------------------------------------------------------- |
| `references/question-design.md`     | Before writing any questions — contains question stem rules and scenario patterns      |
| `references/option-design.md`       | Before writing any options — contains distractor strategies and length-balancing rules |
| `references/answer-distribution.md` | During Step 3 — contains the distribution algorithm and verification method            |
