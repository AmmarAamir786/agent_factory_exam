# Answer Distribution Reference

Algorithm and verification method for ensuring balanced A/B/C/D distribution.

---

## Distribution Algorithm

### Step 1: Determine Total Questions
Count the total number of questions planned (N).

### Step 2: Calculate Targets
Each letter should appear approximately N/4 times:

| Total Questions | Target per Letter | Acceptable Range |
|----------------|-------------------|------------------|
| 20 | 5 | 4-6 |
| 30 | 7-8 | 6-9 |
| 40 | 10 | 9-11 |
| 50 | 12-13 | 11-14 |
| 60 | 15 | 13-17 |

### Step 3: Pre-Assign Using Rotation with Variation

Use a shuffled rotation pattern. Do NOT use simple ABCD cycling (students detect it).

**Method**: Create blocks of 4 questions and assign one of each letter per block, but vary the order within each block:

```
Block 1: D, A, C, B
Block 2: B, D, A, C
Block 3: A, C, B, D
Block 4: C, B, D, A
Block 5: D, A, C, B  (can repeat block patterns if needed)
...
```

For any remaining questions (if N is not divisible by 4), distribute to maintain balance.

### Step 4: Verify Constraints

After assignment, check:

1. **No more than 3 consecutive same-letter answers**
   - Scan the sequence: if you see AAAA or BBBB, swap one with an adjacent question

2. **Each letter within 23-27% of total**
   - Count occurrences of each letter
   - If any letter exceeds range, swap with an underrepresented letter

3. **No obvious patterns**
   - No strict alternation (ABABAB)
   - No predictable cycles (ABCDABCD)
   - Sections should each contain a mix of letters

---

## Verification Checklist

After writing all questions, count and verify:

```
Total questions: ___

A count: ___ (___%)
B count: ___ (___%)
C count: ___ (___%)
D count: ___ (___%)

Longest streak of same letter: ___ (must be ≤ 3)
```

If any check fails, swap answer positions between questions to fix without changing question content — just rearrange which option slot (A/B/C/D) holds the correct answer.

---

## Swapping Technique

To move a correct answer from position B to position D for a given question:
1. Take current option B (correct) and current option D (distractor)
2. Swap their positions: what was B becomes D, what was D becomes B
3. Update the answer key to reflect D as correct
4. Verify the explanation still references the correct letter

This changes nothing about the question's content or difficulty — only the slot assignment.
