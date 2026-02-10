---
name: generating-mcq-exams
description: |
  Generate comprehensive, tough, scenario-based MCQ exams from any source material.
  This skill should be used when users ask to create an exam, generate MCQs, make a quiz,
  build test questions, create assessment questions, or generate practice tests from a file,
  chapter, document, or any study material.
---

# MCQ Exam Generator

Generate scenario-based multiple-choice exams that test deep understanding, not surface recall.

## What This Skill Does
- Reads any source material (markdown, text, PDF, or other documents)
- Identifies every key concept, fact, framework, relationship, and mental model
- Generates tough, scenario-based MCQ questions covering all content
- Produces a single markdown exam file with inline answers and explanations

## What This Skill Does NOT Do
- Generate easy recall-based questions ("What year did X happen?")
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

For each question, follow ALL rules from `references/question-design.md` and `references/option-design.md`. Key rules:

**Questions must be:**
- Scenario-based (present a situation, ask for analysis/judgment)
- Self-contained (never say "the chapter," "the text," "the reading," "according to the passage")
- Embed necessary context directly in the question stem

**Options must be:**
- All four options at comparable word count (within ~20% of each other)
- Every wrong option plausible and detailed — sounds like partial understanding
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
- [ ] Every question is scenario-based or application-based (no "What is X?" recall)
- [ ] Every question is self-contained (no "the chapter says," "according to the text," "the reading mentions")
- [ ] Sufficient context is embedded in each question stem for standalone comprehension
- [ ] All major concepts from the source material are covered

### Option Quality
- [ ] All four options per question have comparable length (within ~20%)
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
