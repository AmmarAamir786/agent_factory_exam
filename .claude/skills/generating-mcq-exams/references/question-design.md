# Question Design Reference

Rules and patterns for writing mixed-strategy MCQ questions — from direct recall to complex scenarios.

---

## Table of Contents

- [Core Rules](#core-rules)
- [Question Type Mix](#question-type-mix)
- [Self-Contained Language](#self-contained-language)
- [Direct Question Patterns](#direct-question-patterns)
- [Conceptual Question Patterns](#conceptual-question-patterns)
- [Scenario Patterns](#scenario-patterns)
- [Bloom&#39;s Taxonomy Targeting](#blooms-taxonomy-targeting)
- [Question Stem Templates](#question-stem-templates)
- [Anti-Patterns to Avoid](#anti-patterns-to-avoid)

---

## Core Rules

1. **Use the right question type for each concept.** Not everything needs a scenario. Definitions and terminology work best as direct questions. Comparisons and "why" questions work as conceptual. Frameworks and decision-making need scenarios. Match the type to the knowledge being tested.
2. **Every question must be self-contained.** A student should understand the question without having read the source material in the same sitting. Embed enough context in the stem itself.
3. **Cover every concept.** Generate as many questions as needed. No artificial limit. Dense topics get more questions. Light topics get fewer. The goal: a student who aces every question has mastered the entire material.
4. **Vary question types within sections.** Alternate between direct, conceptual, and scenario questions. No more than 3 consecutive questions of the same type. This creates cognitive pacing — short punchy questions followed by deeper scenarios.
5. **Target the question type mix.** Across the full exam: ~20% Direct, ~25% Conceptual, ~40% Applied Scenario, ~15% Evaluative Scenario. Track the type internally during planning, but **NEVER include type tags** (e.g., `[Direct]`, `[Conceptual]`) **in the exam output** — the student-facing exam must be clean with no internal metadata visible.
6. **Keep direct/conceptual questions concise.** Stems should be 1-3 sentences. Options should be 5-15 words. Do NOT artificially inflate these to match scenario question length. Short, crisp questions test recall and understanding efficiently.
7. **MANDATORY: Write distractors FIRST, correct answer LAST.** For each question, write the three wrong options first at natural length, then write the correct answer constrained to match their average word count. After writing all four options, count the words in each. If the correct answer is the longest, compress it or expand the distractors — do NOT proceed to the next question until all four options are within ~20% of each other's word count. The correct answer must NEVER be the longest option.

---

## Self-Contained Language

### Banned Phrases

Never use these in question stems:

- "According to the chapter..."
- "The text states that..."
- "As described in the reading..."
- "The author argues..."
- "Based on the passage..."
- "The chapter defines..."
- "This chapter's framework..."
- "The material explains..."

### Replacement Strategy

Instead of referencing the source, embed the concept directly:

```
BAD:  "According to the chapter, what is the Agent Factory paradigm?"
GOOD: "The Agent Factory paradigm describes General Agents as 'the factory floor'
       and Custom Agents as 'the products.' A developer argues that in a real
       factory, the floor doesn't change. Why does the metaphor still hold?"
```

```
BAD:  "The chapter lists four types of convergent evidence. What are they?"
GOOD: "A CTO has evidence from a single Gartner report. Using the convergent
       validation framework (academic, third-party, startup, financial), what
       is the strongest way to build their case?"
```

The question should read as if it's a standalone assessment item — like something you'd find in a professional certification exam.

---

## Question Type Mix

Decide the type for each question **before** writing it. Use this decision tree:

| If the concept is...                                                | Use this type                 |
| ------------------------------------------------------------------- | ----------------------------- |
| A definition, term, named entity, specific number, or key fact      | **Direct**              |
| A comparison, distinction, cause-effect, or "why/purpose" question  | **Conceptual**          |
| A framework, model, process, or decision applied to a new situation | **Applied Scenario**    |
| A counter-argument, evidence evaluation, or multi-concept synthesis | **Evaluative Scenario** |

**Important:** The question type is an internal planning tool. Do NOT include type labels in the exam output.

---

## Direct Question Patterns

Direct questions test foundational knowledge BUT must still be challenging. **Wrap every Direct question in a mini-scenario** so students must apply recall, not just recognize a term. **All options must be 20-35 words** with substantive reasoning — never short throwaway distractors.

### Pattern D1: Scenario-Wrapped Definition

> "A [role] encounters [situation that involves the term]. [Context]. Which explanation most accurately describes [term]?"

Example: "A developer notices Claude Code automatically reads a file at session start without being told. She assumes it's cached from a previous conversation. A colleague says it's a specific file mechanism. Which explanation most accurately describes what's happening?"

- A) Claude Code stores session metadata in a hidden cache directory and replays the most recent project context when a new terminal session is detected in the same working directory
- B) Claude Code automatically detects and reads a CLAUDE.md file in the project root at session start, loading persistent project context that was written once and applies to every future session
- C) Claude Code's underlying model retains a compressed representation of recent interactions that it decompresses when it recognizes the same project directory from previous sessions
- D) Claude Code queries Anthropic's cloud servers for any previously uploaded project context associated with the current directory's unique identifier and injects it into the session

### Pattern D2: Scenario-Wrapped Identification

> "A [role] is evaluating [options/components]. [Specific situation]. Which of the following correctly identifies [concept]?"

### Pattern D3: Scenario-Wrapped Factual Recall

> "[Specific real-world situation involving the data point]. A [role] needs to verify [claim]. What is [the value / the name / the count]?"

### Pattern D4: Scenario-Wrapped Terminology Distinction

> "A team debates whether to use [Term A] or [Term B] for [situation]. What is the key difference that should guide their decision?"

### Direct Question Rules

- Stem: 2-4 sentences with embedded scenario context
- **Options: 20-35 words each, all comparable length with substantive reasoning**
- Every question wrapped in a scenario — never bare "What is X?" format
- Still self-contained (no "according to the chapter")
- **Every distractor must use real terminology and sound plausible to someone who partially understood the material**

---

## Conceptual Question Patterns

Conceptual questions test **understanding** — why something works, how things relate, what the purpose is. Slightly longer than direct but shorter than scenarios.

### Pattern C1: Why/Rationale

> "Why does [concept/approach] [work/matter/exist]?"

Example: "Why is convergent validation preferred over relying on a single evidence source?"

- A) Single sources are always biased toward the vendor's perspective
- B) Multiple independent sources reduce the chance of shared blind spots
- C) Regulatory frameworks require a minimum of four evidence categories
- D) Academic research is more reliable than industry data

### Pattern C2: Comparison

> "How does [Concept A] differ from [Concept B] in terms of [specific dimension]?"

Example: "How does the 'Skipping Incubation' anti-pattern differ from 'Premature Specialization'?"

- A) Skipping Incubation rushes to a Custom Agent; Premature Specialization over-optimizes too early
- B) Skipping Incubation applies to testing; Premature Specialization applies to deployment
- C) They are different names for the same anti-pattern
- D) Skipping Incubation is a design flaw; Premature Specialization is a staffing issue

### Pattern C3: Purpose/Function

> "What is the primary purpose of [framework/component/process]?"

Example: "What is the primary purpose of the Agent Factory paradigm's 'factory floor' metaphor?"

- A) To explain manufacturing automation principles
- B) To distinguish the stable platform (General Agent) from its outputs (Custom Agents)
- C) To describe the physical infrastructure needed for agent deployment
- D) To compare AI agents to traditional software development pipelines

### Pattern C4: Cause-Effect

> "What is the most likely consequence of [action/condition]?"

Example: "What is the most likely consequence of deploying a Custom Agent without sufficient training data variety?"

- A) The agent will consume excessive compute resources
- B) The agent will fail on edge cases not represented in training
- C) The agent will default to General Agent behavior automatically
- D) The agent will require manual retraining every week

### Conceptual Question Rules

- Stem: 2-4 sentences with embedded context or mini-scenario
- **Options: 20-35 words each, each presenting a complete reasoning chain**
- Tests understanding of "why" and "how," not just "what"
- Even conceptual questions should embed a situation: "A team debates X. Developer argues Y. What is the correct reasoning?"
- **Every distractor must present a plausible-sounding explanation using real terminology from the material**

---

## Scenario Patterns

Use these patterns to create scenario-based stems:

### Pattern 1: Role-Based Decision

> "A [role] faces [situation]. Based on [framework/concept], what should they do?"

Example: "A startup founder wants their production system to handle novel situations creatively. Based on the Agent Maturity Model, what does this suggest?"

### Pattern 2: Counter-Argument

> "[Claim or statistic]. A [person] argues [counter-position]. What is the best response?"

Example: "The GDPval Benchmark shows a 49% win rate against human experts. A researcher argues humans are still better. What is the most accurate counter-argument?"

### Pattern 3: Anti-Pattern Identification

> "A team does [specific action sequence]. What anti-pattern does this represent?"

Example: "A team builds a Custom Agent for invoice processing without any incubation phase. Six months later, 30% of edge cases are mishandled. Which anti-pattern is this?"

### Pattern 4: Analogy Challenge

> "[Metaphor/analogy is stated]. A critic argues [flaw in the analogy]. Why does it still hold (or not)?"

### Pattern 5: Application to New Context

> "A [new scenario not in the source] requires a decision. Applying [framework from source], which approach is correct?"

### Pattern 6: Distinguishing Similar Concepts

> "[Two concepts that are easily confused]. In [specific situation], which one applies and why?"

### Pattern 7: Evidence Evaluation

> "[A claim with specific supporting evidence]. How should this evidence be assessed using [framework]?"

---

## Bloom's Taxonomy Targeting

Use ALL six levels, matched to question types:

| Level                | Question Type                            | Maps To                        | Example Verb                                          |
| -------------------- | ---------------------------------------- | ------------------------------ | ----------------------------------------------------- |
| 1. Remember          | Recall facts, terms, definitions         | **Direct**               | "What does X refer to..."                             |
| 2. Understand        | Explain, compare, interpret              | **Direct / Conceptual**  | "Why does X differ from Y..."                         |
| 3. Apply             | Use a framework in a new scenario        | **Conceptual / Applied** | "Based on the diagnostic criteria, which approach..." |
| 4. Analyze           | Break down a situation into components   | **Applied**              | "What specific problems will they encounter..."       |
| 5. Evaluate          | Judge or defend a position               | **Evaluative**           | "Why is this reasoning flawed..."                     |
| 6. Create/Synthesize | Combine concepts to solve novel problems | **Evaluative**           | "What is the strongest way to build their case..."    |

Levels 1-2 are valid for ~20% of the exam (Direct questions). The majority (~55%) should still target levels 3-5.

---

## Question Stem Templates

### Direct Templates

```
"What does [term] refer to?"
```

```
"Which of the following is [a characteristic / component / example] of [concept]?"
```

```
"How many [items] does [framework] define?"
```

```
"What is the key difference between [Term A] and [Term B]?"
```

### Conceptual Templates

```
"Why is [approach A] preferred over [approach B]?"
```

```
"What is the primary purpose of [framework/component]?"
```

```
"What is the most likely consequence of [action/condition]?"
```

```
"How does [Concept A] relate to [Concept B]?"
```

### Applied Scenario Templates

```
"[Framework name] distinguishes between [X] and [Y]. A [role] is in [situation].
Based on the diagnostic criteria, they should..."
```

```
"In the [process name], Phase [N] involves [brief description]. A team
[does something wrong related to that phase]. The likely consequence is..."
```

```
"[Concept A] is described as [trait]. [Concept B] is described as [opposite trait].
A [role] wants [specific outcome]. What does this suggest?"
```

```
"A [real-world entity] currently [does X]. A [new approach] would differ because..."
```

### Evaluative Scenario Templates

```
"[Statistic with source]. A [skeptic/critic] argues [counter-interpretation].
The most accurate response is..."
```

```
"[Claim A] and [Claim B] appear to contradict each other. What resolves this tension?"
```

---

## Anti-Patterns to Avoid

| Anti-Pattern                          | Why It's Bad                                                         | Fix                                                                          |
| ------------------------------------- | -------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Only scenario questions               | Causes fatigue, inflates length, not always the right tool           | Use the question type mix (~20% Direct, ~25% Conceptual, ~55% Scenario)      |
| "Which of these is true?"             | Vague, no focus                                                      | Ask about a specific concept or distinction                                  |
| "All of the above"                    | Tests guessing, not knowledge                                        | Use 4 distinct options                                                       |
| "None of the above"                   | Frustrating, unclear learning                                        | Use 4 testable options                                                       |
| Negatively worded ("Which is NOT...") | Confusing, error-prone                                               | Use positive framing except for EXCEPT questions                             |
| Double negatives                      | Confusing                                                            | Rewrite in positive form                                                     |
| Trivially true options                | Too easy to eliminate                                                | Make all options plausible                                                   |
| Trick questions                       | Tests attention, not understanding                                   | Test genuine comprehension                                                   |
| Artificially long Direct questions    | Padding short questions to match scenario length wastes student time | Keep Direct questions naturally short (1-2 sentence stem, 5-15 word options) |
| All Direct, no scenarios              | Doesn't test application or judgment                                 | Ensure ~55% are Applied/Evaluative scenarios                                 |
