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

| File | When to Read |
|------|--------------|
| `references/question-design.md` | Before writing any questions — contains question stem rules and scenario patterns |
| `references/option-design.md` | Before writing any options — contains distractor strategies and length-balancing rules |
| `references/answer-distribution.md` | During Step 3 — contains the distribution algorithm and verification method |
