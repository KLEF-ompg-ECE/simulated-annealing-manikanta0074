# Assignment 1 — Simulated Annealing: Exam Timetable Scheduling
## Observation Report

**Student Name  :** __Manikanta.A__________  
**Student ID    :** _____2310040074______ 
**Date Submitted:** _______03/04/2026____ 

---

## How to Submit

1. Run each experiment following the instructions below
2. Fill in every answer box — do not leave placeholders
3. Make sure the `plots/` folder contains all required images
4. Commit this README and the `plots/` folder to your GitHub repo

---

## Before You Begin — Read the Code

Open `sa_timetable.py` and read through it. Then answer these questions.

**Q1. What does `count_clashes()` measure? What value means a perfect timetable?**

```
count_clashes() counts the total number of times a student is assigned two or more
exams in the same time slot across all students. It loops through every student's
exam list and checks whether any two of their exams share the same slot. A return
value of 0 means a perfect timetable — no student has any conflicting exams.
```

**Q2. What does `generate_neighbor()` do? How is the new timetable different from the current one?**

```
generate_neighbor() creates a slightly modified copy of the current timetable by
picking one exam at random and reassigning it to a different time slot (never the
same slot it was already in). The new timetable is identical to the current one
except for that single exam's slot assignment, making it a small, local change.
```

**Q3. In `run_sa()`, there is this line:**
```python
if delta < 0 or random.random() < math.exp(-delta / T):
```
**What does this line decide? Why does SA sometimes accept a worse solution?**

```
This line decides whether to move to the neighbouring timetable or stay with the
current one. If the neighbour is better (delta < 0), it is always accepted.
If it is worse, it is still accepted with a probability of e^(-delta/T) — this
probability is higher when the temperature T is high and shrinks as T cools.
SA accepts worse solutions occasionally so it can escape local optima and continue
exploring the search space, avoiding getting permanently stuck in a suboptimal spot.
```

---

## Experiment 1 — Baseline Run

**Instructions:** Run the program without changing anything.
```bash
python sa_timetable.py
```

**Fill in this table:**

| Metric | Your result |
|--------|-------------|
| Number of iterations completed | 1379 |
| Clashes at iteration 1 | 12 |
| Final best clashes | 3 |
| Did SA reach 0 clashes? (Yes / No) | No |

**Copy the printed timetable output here:**
```
  Final Timetable
------------------------------------------
  Slot 1:  Geography
  Slot 2:  Chemistry, English
  Slot 3:  History, Computer Science, Economics
  Slot 4:  Biology, Statistics
  Slot 5:  Mathematics, Physics
------------------------------------------
  Total clashes : 3
```

**Look at `plots/experiment_1.png` and describe what you see (2–3 sentences).**  
*Where does the biggest drop in clashes happen? Does the curve flatten out?*
```
The biggest drop in clashes occurs within the first 200 iterations, where the clash
count falls rapidly from 12 down toward 3 while the temperature is still relatively
high and the algorithm is exploring freely. After that early steep decline, the curve
flattens completely and stays at 3 clashes for the remainder of the run. The
temperature plot shows a smooth exponential decay, confirming the slow, steady
cooling schedule of rate 0.995.
```

---

## Experiment 2 — Effect of Cooling Rate

**Instructions:** In `sa_timetable.py`, find the `# EXPERIMENT 2` block in `__main__`.  
Copy it three times and run with `cooling_rate` = **0.80**, **0.95**, and **0.995**.  
Save plots as `experiment_2a.png`, `experiment_2b.png`, `experiment_2c.png`.

**Results table:**

| cooling_rate | Final clashes | Iterations completed | Reached 0 clashes? |
|-------------|---------------|----------------------|--------------------|
| 0.80        | 8             | 31                   | No                 |
| 0.95        | 3             | 135                  | No                 |
| 0.995       | 3             | 1379                 | No                 |

**Compare the three plots. What do you notice about how fast vs slow cooling affects the result? (3–4 sentences)**  
*Hint: Fast cooling = temperature drops quickly. Does it have time to explore well?*
```
With a fast cooling rate of 0.80, the temperature drops below the minimum threshold
in just 31 iterations, giving the algorithm almost no time to explore the search
space — it gets stuck at 8 clashes, a poor result. At 0.95, the algorithm runs for
135 iterations and manages to reach 3 clashes, matching the quality of the slowest
schedule despite being about 10x faster. The slowest rate of 0.995 runs for 1379
iterations with the most thorough exploration but achieves the same 3-clash result
as 0.95 on this problem. This shows that cooling too fast is harmful, but beyond a
certain point, slowing down further does not always improve the final answer.
```

**Which cooling_rate gave the best result? Why do you think that is?**
```
Both 0.95 and 0.995 gave the best result of 3 final clashes. However, 0.95 is more
efficient because it reaches the same quality solution in only 135 iterations instead
of 1379. The rate 0.80 performed worst because the temperature fell so quickly that
the algorithm froze into a bad solution before it could explore enough of the search
space to find improvements. A moderate-to-slow cooling rate gives SA enough time at
high temperatures to escape local optima while still converging to a good solution.
```

---

## Summary

**Complete this table with your best result from each experiment:**

| Experiment | Key setting | Final clashes | Main finding in one sentence |
|------------|-------------|---------------|------------------------------|
| 1 — Baseline | cooling_rate = 0.995 | 3 | SA reduced clashes from 12 to 3 with a slow cooling schedule, showing that gradual temperature decay allows effective exploration. |
| 2 — Cooling rate | cooling_rate = 0.95 | 3 | A moderate cooling rate of 0.95 matched the best result 10x faster, while a rate of 0.80 cooled too fast and got stuck at 8 clashes. |

**In your own words — what is the most important thing you learned about Simulated Annealing from these experiments? (3–5 sentences)**
```
The most important thing I learned is that the cooling rate is a critical parameter
that controls the trade-off between exploration and exploitation in Simulated
Annealing. If the temperature drops too fast, the algorithm behaves almost like a
greedy search — it accepts the first improvement it finds and gets permanently stuck
in a local optimum, as seen with cooling_rate=0.80 ending at 8 clashes. A slower
cooling rate keeps the temperature high for longer, allowing SA to occasionally
accept worse solutions and escape those traps. I also learned that there is a point
of diminishing returns: slowing the cooling rate from 0.95 to 0.995 used ten times
more iterations but produced no better final answer, so choosing the right cooling
rate is about balancing solution quality with computational effort.
```

---

## Submission Checklist

- [done] Student name and ID filled in
- [done] Q1, Q2, Q3 answered
- [done] Experiment 1: table filled, timetable pasted, plot observation written
- [done] Experiment 2: results table filled (3 rows), observation and answer written
- [done] Summary table completed and reflection written
- [done] `plots/` contains: `experiment_1.png`, `experiment_2a.png`, `experiment_2b.png`, `experiment_2c.png`
