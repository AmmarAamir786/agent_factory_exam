# Question Design Reference

Rules and patterns for writing tough, scenario-based MCQ questions.

---

## Table of Contents
- [Core Rules](#core-rules)
- [Self-Contained Language](#self-contained-language)
- [Scenario Patterns](#scenario-patterns)
- [Bloom's Taxonomy Targeting](#blooms-taxonomy-targeting)
- [Question Stem Templates](#question-stem-templates)
- [Anti-Patterns to Avoid](#anti-patterns-to-avoid)

---

## Core Rules

1. **Every question must present a situation, then ask for analysis or judgment.** Never ask "What is X?" — always ask "Given situation Y, what is the correct interpretation/action/analysis?"

2. **Every question must be self-contained.** A student should understand the question without having read the source material in the same sitting. Embed enough context in the stem itself.

3. **Cover every concept.** Generate as many questions as needed. No artificial limit. Dense topics get more questions. Light topics get fewer. The goal: a student who aces every question has mastered the entire material.

4. **Vary difficulty within sections.** Mix application-level and evaluation-level questions. Not every question needs maximum difficulty — but none should be trivial recall.

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

Target levels 3-6 (Application through Evaluation). Avoid levels 1-2 (Remember, Understand) as standalone questions.

| Level | Question Type | Example Verb |
|-------|--------------|--------------|
| 3. Apply | Use a framework in a new scenario | "Based on the diagnostic criteria, which approach..." |
| 4. Analyze | Break down a situation into components | "What specific problems will they encounter..." |
| 5. Evaluate | Judge or defend a position | "Why is this reasoning flawed..." |
| 6. Create/Synthesize | Combine concepts to solve novel problems | "What is the strongest way to build their case..." |

---

## Question Stem Templates

### For Frameworks/Models
```
"[Framework name] distinguishes between [X] and [Y]. A [role] is in [situation].
Based on the diagnostic criteria, they should..."
```

### For Statistics/Data
```
"[Statistic with source]. A [skeptic/critic] argues [counter-interpretation].
The most accurate response is..."
```

### For Processes/Phases
```
"In the [process name], Phase [N] involves [brief description]. A team
[does something wrong related to that phase]. The likely consequence is..."
```

### For Comparisons
```
"[Concept A] is described as [trait]. [Concept B] is described as [opposite trait].
A [role] wants [specific outcome]. What does this suggest?"
```

### For Definitions/Concepts
```
"A [real-world entity] currently [does X]. A [new approach] would differ because..."
```

---

## Anti-Patterns to Avoid

| Anti-Pattern | Why It's Bad | Fix |
|-------------|-------------|-----|
| "What is X?" | Pure recall, no thinking | Embed in a scenario |
| "Which of these is true?" | Vague, no scenario | Add a specific situation |
| "All of the above" | Tests guessing, not knowledge | Use 4 distinct options |
| "None of the above" | Frustrating, unclear learning | Use 4 testable options |
| Negatively worded ("Which is NOT...") | Confusing, error-prone | Use positive framing except for EXCEPT questions |
| Double negatives | Confusing | Rewrite in positive form |
| Trivially true options | Too easy to eliminate | Make all options plausible |
| Trick questions | Tests attention, not understanding | Test genuine comprehension |
