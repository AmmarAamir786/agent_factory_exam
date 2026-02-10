# Option Design Reference

Rules and strategies for writing plausible, balanced MCQ options (distractors).

---

## Table of Contents
- [The Cardinal Rule: Length Balance](#the-cardinal-rule-length-balance)
- [Distractor Quality Standards](#distractor-quality-standards)
- [Distractor Generation Strategies](#distractor-generation-strategies)
- [Option Formatting Rules](#option-formatting-rules)
- [Anti-Patterns to Avoid](#anti-patterns-to-avoid)
- [Length Balancing Techniques](#length-balancing-techniques)

---

## The Cardinal Rule: Length Balance

**The correct answer must NEVER be identifiable by being the longest option.**

This is the single most common flaw in MCQ design. Students learn to pick the longest, most detailed answer — and when it's always correct, the exam tests test-taking skill, not knowledge.

### Measurement Rule
All four options for a given question must be within ~20% word count of each other. If the correct answer is 30 words, every distractor must be 24-36 words.

### How to Achieve This
1. Write the correct answer first at natural length
2. Expand each distractor to match that length with plausible detail
3. If the correct answer is naturally long, compress it and add detail to distractors
4. Never pad with filler — added length must be substantive, plausible reasoning

---

## Distractor Quality Standards

Every wrong option must pass this test:

> "Would a student who partially understood the material — but missed a key nuance — choose this option?"

If the answer is "no, obviously not," the distractor needs rework.

### Quality Levels

| Level | Description | Example |
|-------|-------------|---------|
| **Bad** | Obviously wrong, no one would choose it | "Because the sky is green" |
| **Weak** | Related to the topic but clearly wrong | "AI was invented in 2025" |
| **Good** | Plausible if you missed a distinction | "49% means AI already surpasses humans" (confuses parity with superiority) |
| **Excellent** | Requires precise understanding to reject | Uses correct terminology, references real concepts, but applies them incorrectly or to the wrong context |

Target: ALL distractors at "Good" or "Excellent" level.

---

## Distractor Generation Strategies

### Strategy 1: Partial Truth
Take a true statement and extend it with a false conclusion.

```
Correct: "Scale amplifies the 10% gain to 8,000 equivalent developers"
Distractor: "The 10% figure only captures direct coding speed; indirect gains push it to 40%"
(The 10% part references real data, but the 40% extrapolation is fabricated)
```

### Strategy 2: Common Misconception
Represent what someone with surface-level understanding might believe.

```
Correct: "Convergent validation requires independent source categories"
Distractor: "Multiple vendor benchmarks qualify as convergent validation since competing incentives cancel bias"
(Sounds logical but misses that vendors are a single category)
```

### Strategy 3: Adjacent Concept
Use a real concept from the material but apply it to the wrong situation.

```
Correct: "This is Skipping Incubation"
Distractor: "This is Premature Specialization — they built before sufficient user data existed"
(Premature Specialization is a real concept, but it means something different)
```

### Strategy 4: Overextension
Take the correct reasoning one step too far.

```
Correct: "The claim is insufficient without independent verification"
Distractor: "The claim should be rejected outright — vendor evidence is inherently unreliable"
(Goes from "insufficient" to "reject outright" — too strong)
```

### Strategy 5: Correct Mechanism, Wrong Cause
Describe a real outcome but attribute it to the wrong reason.

```
Correct: "Enterprise companies bet on ground-up platform redesign for AI agents"
Distractor: "The acquisition was defensive — preventing competitors from acquiring the target"
(Acquisitions can be defensive, but the source specifically says otherwise)
```

### Strategy 6: Plausible Alternative Framework
Present a different but reasonable-sounding analytical framework.

```
Correct: "Software disrupts itself because the tools that build it change how it's built"
Distractor: "The speed difference makes them fundamentally different — software in 3-5 years vs auto in 30-50"
(Speed IS different, but it's not the core distinction the material makes)
```

---

## Option Formatting Rules

### Structure
```markdown
- A) [Option text — complete sentence or clause, comparable length to other options]
- B) [Option text — complete sentence or clause, comparable length to other options]
- C) [Option text — complete sentence or clause, comparable length to other options]
- D) [Option text — complete sentence or clause, comparable length to other options]
```

### Consistency Rules
1. All options must be grammatically parallel (all start the same way)
2. All options must be complete thoughts (no fragments)
3. All options must answer the question asked (not tangential)
4. Options should not overlap or contain each other
5. Options should be mutually exclusive

---

## Anti-Patterns to Avoid

| Anti-Pattern | Problem | Fix |
|-------------|---------|-----|
| Correct answer 2x longer than distractors | Instantly identifiable | Expand distractors or compress correct |
| Distractors that are obviously absurd | No one picks them, reduces to 2-3 options | Make them plausible with partial truths |
| "All of the above" / "None of the above" | Tests guessing patterns, not knowledge | Use 4 distinct substantive options |
| Using "always" or "never" in distractors only | Students learn to avoid absolutes | Use absolute language consistently or not at all |
| Correct answer uses hedging ("may," "can," "often") | Pattern signals the right answer | Use similar language across all options |
| One option much more specific than others | Specificity signals correctness | Match specificity levels |
| Distractors from completely different topics | No partial-knowledge student would pick them | Keep all options within the same concept area |
| Joke or throwaway options | Disrespects the student, reduces effective choices | Every option must be a serious answer |

---

## Length Balancing Techniques

### Technique 1: Add Reasoning to Short Distractors
```
TOO SHORT: "Because AI is faster"
BALANCED:  "AI agents offer the same features at a lower per-user price point,
            undercutting the incumbent on cost while maintaining feature parity
            across all workflows"
```

### Technique 2: Add Consequence to Short Distractors
```
TOO SHORT: "The requirements are unclear"
BALANCED:  "The goal was likely too vague, causing the agent to make assumptions;
            a more detailed specification would have produced correct output on
            the first attempt"
```

### Technique 3: Add Mechanism to Short Distractors
```
TOO SHORT: "It won't work at scale"
BALANCED:  "The General Agent will hit API rate limits and begin dropping requests,
            creating a degraded experience that worsens during peak hours and
            eventually causes complete service outages"
```

### Technique 4: Compress a Long Correct Answer
```
TOO LONG:  "The shift is in the developer's role where you evaluate AI output
            quality and correctness as a quality control process rather than
            hunting for bugs in code you wrote yourself and testing remains
            essential but the mindset changes from debugging to validation"
COMPRESSED: "The shift is in the developer's role — you evaluate AI output
             quality as a quality control process, not hunt for bugs in code
             you wrote yourself; testing remains essential"
```

### Final Check
After writing all options for a question, count the words in each. If any option deviates more than 20% from the average, rebalance.
