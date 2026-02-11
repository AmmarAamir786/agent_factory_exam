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
- [Bloom's Taxonomy Targeting](#blooms-taxonomy-targeting)
- [Question Stem Templates](#question-stem-templates)
- [Anti-Patterns to Avoid](#anti-patterns-to-avoid)

---

## Core Rules

1. **Use the right question type for each concept.** Not everything needs a scenario. Definitions and terminology work best as direct questions. Comparisons and "why" questions work as conceptual. Frameworks and decision-making need scenarios. Match the type to the knowledge being tested.

2. **Every question must be self-contained.** A student should understand the question without having read the source material in the same sitting. Embed enough context in the stem itself.

3. **Cover every concept.** Generate as many questions as needed. No artificial limit. Dense topics get more questions. Light topics get fewer. The goal: a student who aces every question has mastered the entire material.

4. **Vary question types within sections.** Alternate between direct, conceptual, and scenario questions. No more than 3 consecutive questions of the same type. This creates cognitive pacing — short punchy questions followed by deeper scenarios.

5. **Target the question type mix.** Across the full exam: ~20% Direct, ~25% Conceptual, ~40% Applied Scenario, ~15% Evaluative Scenario. Tag each question with its type (e.g., `[Direct]`, `[Conceptual]`, `[Applied]`, `[Evaluative]`) for verification.

6. **Keep direct/conceptual questions concise.** Stems should be 1-3 sentences. Options should be 5-15 words. Do NOT artificially inflate these to match scenario question length. Short, crisp questions test recall and understanding efficiently.

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

| If the concept is... | Use this type | Tag |
|----------------------|---------------|-----|
| A definition, term, named entity, specific number, or key fact | **Direct** | `[Direct]` |
| A comparison, distinction, cause-effect, or "why/purpose" question | **Conceptual** | `[Conceptual]` |
| A framework, model, process, or decision applied to a new situation | **Applied Scenario** | `[Applied]` |
| A counter-argument, evidence evaluation, or multi-concept synthesis | **Evaluative Scenario** | `[Evaluative]` |

---

## Direct Question Patterns

Direct questions test whether a student knows foundational facts. Keep them **short and crisp** — 1-2 sentence stems, 5-15 word options.

### Pattern D1: Definition
> "What does [term] refer to in the context of [domain]?"

Example: "What does 'incubation phase' refer to in the Agent Maturity Model?"
- A) A testing period before production deployment
- B) The period where a General Agent handles tasks before a Custom Agent is built
- C) A mandatory security review before agent release
- D) The initial training phase for a new language model

### Pattern D2: Identification
> "Which of the following is [a characteristic / an example / a component] of [concept]?"

Example: "Which of the following is a level in the Agent Maturity Model?"
- A) Autonomous orchestration
- B) Supervised delegation
- C) Conversational AI
- D) Predictive analytics

### Pattern D3: Factual Recall
> "[Specific claim with a number or named entity]. What is [the value / the name / the count]?"

Example: "In the GDPval Benchmark, what win rate did AI agents achieve against human experts?"
- A) 34%
- B) 49%
- C) 62%
- D) 75%

### Pattern D4: Terminology Distinction
> "What is the key difference between [Term A] and [Term B]?"

Example: "What is the key difference between a General Agent and a Custom Agent?"
- A) General Agents use LLMs; Custom Agents use rule-based systems
- B) General Agents handle broad tasks; Custom Agents are optimized for specific workflows
- C) General Agents are open-source; Custom Agents are proprietary
- D) General Agents run locally; Custom Agents run in the cloud

### Direct Question Rules
- Stem: 1-2 sentences maximum
- Options: 5-15 words each, all comparable length
- No scenario setup needed — just ask directly
- Still self-contained (no "according to the chapter")
- Distractors must be plausible within the domain — not absurd

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
- Stem: 1-3 sentences
- Options: 10-20 words each, explaining reasoning briefly
- Tests understanding of "why" and "how," not just "what"
- No elaborate scenario setup — focus on the relationship between concepts

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

| Level | Question Type | Maps To | Example Verb |
|-------|--------------|---------|--------------|
| 1. Remember | Recall facts, terms, definitions | **Direct** | "What does X refer to..." |
| 2. Understand | Explain, compare, interpret | **Direct / Conceptual** | "Why does X differ from Y..." |
| 3. Apply | Use a framework in a new scenario | **Conceptual / Applied** | "Based on the diagnostic criteria, which approach..." |
| 4. Analyze | Break down a situation into components | **Applied** | "What specific problems will they encounter..." |
| 5. Evaluate | Judge or defend a position | **Evaluative** | "Why is this reasoning flawed..." |
| 6. Create/Synthesize | Combine concepts to solve novel problems | **Evaluative** | "What is the strongest way to build their case..." |

Levels 1-2 are valid for ~20% of the exam (Direct questions). The majority (~55%) should still target levels 3-5.

---

## Question Stem Templates

### Direct Templates

```
[Direct] "What does [term] refer to?"
```
```
[Direct] "Which of the following is [a characteristic / component / example] of [concept]?"
```
```
[Direct] "How many [items] does [framework] define?"
```
```
[Direct] "What is the key difference between [Term A] and [Term B]?"
```

### Conceptual Templates

```
[Conceptual] "Why is [approach A] preferred over [approach B]?"
```
```
[Conceptual] "What is the primary purpose of [framework/component]?"
```
```
[Conceptual] "What is the most likely consequence of [action/condition]?"
```
```
[Conceptual] "How does [Concept A] relate to [Concept B]?"
```

### Applied Scenario Templates

```
[Applied] "[Framework name] distinguishes between [X] and [Y]. A [role] is in [situation].
Based on the diagnostic criteria, they should..."
```
```
[Applied] "In the [process name], Phase [N] involves [brief description]. A team
[does something wrong related to that phase]. The likely consequence is..."
```
```
[Applied] "[Concept A] is described as [trait]. [Concept B] is described as [opposite trait].
A [role] wants [specific outcome]. What does this suggest?"
```
```
[Applied] "A [real-world entity] currently [does X]. A [new approach] would differ because..."
```

### Evaluative Scenario Templates

```
[Evaluative] "[Statistic with source]. A [skeptic/critic] argues [counter-interpretation].
The most accurate response is..."
```
```
[Evaluative] "[Claim A] and [Claim B] appear to contradict each other. What resolves this tension?"
```

---

## Anti-Patterns to Avoid

| Anti-Pattern | Why It's Bad | Fix |
|-------------|-------------|-----|
| Only scenario questions | Causes fatigue, inflates length, not always the right tool | Use the question type mix (~20% Direct, ~25% Conceptual, ~55% Scenario) |
| "Which of these is true?" | Vague, no focus | Ask about a specific concept or distinction |
| "All of the above" | Tests guessing, not knowledge | Use 4 distinct options |
| "None of the above" | Frustrating, unclear learning | Use 4 testable options |
| Negatively worded ("Which is NOT...") | Confusing, error-prone | Use positive framing except for EXCEPT questions |
| Double negatives | Confusing | Rewrite in positive form |
| Trivially true options | Too easy to eliminate | Make all options plausible |
| Trick questions | Tests attention, not understanding | Test genuine comprehension |
| Artificially long Direct questions | Padding short questions to match scenario length wastes student time | Keep Direct questions naturally short (1-2 sentence stem, 5-15 word options) |
| All Direct, no scenarios | Doesn't test application or judgment | Ensure ~55% are Applied/Evaluative scenarios |
