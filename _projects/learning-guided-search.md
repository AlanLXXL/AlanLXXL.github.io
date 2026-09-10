---
title: "Learning-Guided Memetic Search for Combinatorial Optimisation"
order: 2
period: "Sep 2025 – May 2026"
role: "Solo final-year research project · University of Nottingham"
excerpt: "My final-year research project: a Java framework in which a probabilistic model learns from the best solutions found so far and steers local search and crossover toward the bits it is least sure about. Evaluated on 24 MAX-SAT and 0–1 Knapsack instances, 31 runs per configuration, under budgets that give every algorithm the same number of fitness evaluations."
result: "Won or tied 11 of 12 MAX-SAT benchmark instances against four baselines under an equal budget of 755,678 evaluations, average rank 1.17 of 5. On 0–1 Knapsack the advantage did not transfer, and the write-up explains why."
stack:
  - Java 21
  - Python
  - pandas
  - matplotlib
  - LaTeX
links:
  - label: "Dissertation (PDF)"
    url: "/assets/files/xing-lu-fyp-dissertation.pdf"
image: /assets/images/projects/fyp/pv-heatmap-thumb.png
image_alt: "Detail of a PBIL probability-vector heatmap: 250 bits over 60 generations turning from pale to saturated red and blue as the model commits"
stats:
  - value: "11"
    label: "of 12 MAX-SAT instances won or tied"
    note: "against four baselines, identical evaluation budget"
  - value: "1.17"
    label: "average rank, 1 = best"
    note: "next best 2.25; standalone PBIL 3.92"
  - value: "98%"
    label: "of crossover gated shut by generation 40"
    note: "eligible bits fall from 250 to about 5 at τ = 0.05 as the model gains confidence"
  - value: "31"
    label: "runs per configuration"
    note: "24 instances in two domains, paired seeds, every ablation"
highlights:
  - title: "Uncertainty beats preference"
    text: "Ordering hill climbing by how *unsure* the model is about each bit ranked 1.48 across 12 instances, ahead of the random-order baseline (1.78) and a variant that follows the model's preference (2.74). The change is one comparator in the visit order; the evaluation count is identical."
  - title: "Crossover that switches itself off"
    text: "Entropy-restricted uniform crossover swaps a bit only when the model's confidence in it is below τ = 0.05. Eligible bits fall from 250 to about 5 within 40 generations, so recombination fades exactly as the search stabilises. It was the best crossover configuration, rank 1.88 against 3.13 for plain uniform crossover."
  - title: "A fair budget and an honest negative"
    text: "Every algorithm got the same 755,678 fitness evaluations, which removes the hidden advantage memetic algorithms get from fixed-generation comparisons. The framework still won or tied 11 of 12 MAX-SAT instances. On 0–1 Knapsack a plain genetic algorithm came first and the framework near last, and I explain the mechanism rather than bury it."
---

## Problem

Combinatorial optimisation problems such as MAX-SAT and 0–1 Knapsack sit behind scheduling, allocation and routing, and their search spaces grow exponentially with size. Evolutionary and memetic algorithms handle them well in practice, but their operators are blind: uniform crossover swaps bits at random, and a hill climber visits bits in a random order. Meanwhile, Population-Based Incremental Learning (PBIL) learns a probability vector from good solutions, one probability per bit, but it is normally used only to sample new solutions, never to inform what the operators do.

My research question was whether a lightweight learned model can tell local search and crossover *where* to spend a limited evaluation budget, and whether the answer holds across two structurally different binary domains. The answer turned out to be yes on MAX-SAT, no on Knapsack, and the reason why is the most useful thing in the project.

## The framework

<div class="arch">
  <div class="arch__layer">
    <div class="arch__layer-label">Evolutionary loop &middot; one generation, population of 50</div>
    <div class="arch__boxes">
      <div class="arch__box"><strong>Tournament selection</strong><span>size 3, picks two parents</span></div>
      <div class="arch__box"><strong>ER-UXO crossover</strong><span>swaps a bit only where the model is still unsure</span></div>
      <div class="arch__box"><strong>Bit-flip mutation</strong><span>rate 1/n</span></div>
      <div class="arch__box"><strong>Entropy-guided DBHC</strong><span>local search that visits the least certain bits first</span></div>
      <div class="arch__box"><strong>Elitist replacement</strong><span>trans-generational; the best solutions survive</span></div>
    </div>
  </div>
  <div class="arch__flow">
    <span class="arch__flow-item">learn: best solution &rarr; probability vector, &alpha; = 0.01 &darr;</span>
    <span class="arch__flow-item">&uarr; guide: per-bit uncertainty |p<sub>j</sub> &minus; 0.5| &rarr; operators</span>
  </div>
  <div class="arch__layer arch__layer--accent">
    <div class="arch__layer-label">PBIL probability vector &middot; one probability per bit, clipped to [0.05, 0.95]</div>
    <div class="arch__boxes">
      <div class="arch__box"><strong>Update</strong><span><code>p[j] = (1 &minus; &alpha;)&middot;p[j] + &alpha;&middot;elite[j]</code> after replacement, learning from the single best individual (K = 1)</span></div>
      <div class="arch__box"><strong>Uncertainty</strong><span><code>|p[j] &minus; 0.5|</code>, ascending, sets the local-search visit order</span></div>
      <div class="arch__box"><strong>Confidence gate</strong><span><code>2&middot;|p[j] &minus; 0.5| &lt; &tau;</code> decides whether crossover may touch bit j</span></div>
    </div>
  </div>
  <div class="arch__flow">
    <span class="arch__flow-item">one bitstring representation, two objective functions &darr;</span>
  </div>
  <div class="arch__layer">
    <div class="arch__layer-label">Problem domains</div>
    <div class="arch__boxes">
      <div class="arch__box"><strong>MAX-SAT</strong><span>minimise unsatisfied clauses; the 12 instances of the CHeSC 2011 benchmark, 250 to 744 variables, 1,000 to 3,500 clauses</span></div>
      <div class="arch__box"><strong>0&ndash;1 Knapsack</strong><span>maximise profit with a capacity penalty, &lambda; = 2 &times; mean profit-to-weight density; 12 Pisinger instances, 50 to 10,000 items in four correlation classes</span></div>
    </div>
  </div>
</div>

Two decisions shape everything above. First, the model never replaces the population; it only *reorders and gates* what the operators already do, so every guided operator can be ablated against its unguided twin with an identical number of fitness evaluations. Second, everything is a component injected at construction: a search method is a crossover, a mutation, a local search, a parent selection, a replacement and an optional PBIL model. Swapping the local search for a no-op turns the memetic algorithm into a plain GA; swapping the crossover class turns uniform crossover into ER-UXO. Each experiment is a `Config` of constants, seeds and instances plus a `Runner`; one parent seed derives every run seed, so run *k* of one algorithm is paired with run *k* of every other, and runs fan out across CPU cores.

## What I built

### Entropy-guided local search

Davis's Bit Hill Climbing (DBHC) flips bits one at a time in a random order and keeps any flip that does not make the solution worse. I built two guided versions that keep the loop, the acceptance rule and the evaluation count, and change only the visit order:

- **Conflict-guided.** Visit first the bits where the current solution disagrees with the model's preference.
- **Entropy-guided.** Visit first the bits where the model is least certain, in ascending order of how far p<sub>j</sub> is from 0.5.

```java
// DavissBitHillClimbing.java
// baseline: random visit order
int[] perm =
    ArrayMethods.shuffle(variableIndices, this.random);

// PBIL_Guided_DBHC_Entropy_IE.java
// proposed: least certain bits first
Arrays.sort(indices, Comparator.comparingDouble(
        (Integer i) -> Math.abs(pVector[i] - 0.5)));
```

<div class="dotplot">
  <div class="dotplot__title">MAX-SAT local-search ablation &middot; average paired rank over 12 instances &times; 31 runs (1 = best)</div>
  <div class="dotplot__row dotplot__row--accent">
    <span class="dotplot__label">Entropy-guided DBHC</span>
    <span class="dotplot__track"><span class="dotplot__dot" style="left:16%"></span></span>
    <span class="dotplot__value">1.48</span>
  </div>
  <div class="dotplot__row">
    <span class="dotplot__label">DBHC, random order</span>
    <span class="dotplot__track"><span class="dotplot__dot" style="left:26%"></span></span>
    <span class="dotplot__value">1.78</span>
  </div>
  <div class="dotplot__row">
    <span class="dotplot__label">Conflict-guided DBHC</span>
    <span class="dotplot__track"><span class="dotplot__dot" style="left:58%"></span></span>
    <span class="dotplot__value">2.74</span>
  </div>
  <div class="dotplot__row">
    <span class="dotplot__label">Steepest-descent hill climbing</span>
    <span class="dotplot__track"><span class="dotplot__dot" style="left:100%"></span></span>
    <span class="dotplot__value">4.00</span>
  </div>
  <div class="dotplot__axis">
    <span class="dotplot__label"></span>
    <span class="dotplot__ticks"><span style="left:0">1</span><span style="left:33.3%">2</span><span style="left:66.7%">3</span><span style="left:100%">4</span></span>
    <span class="dotplot__value"></span>
  </div>
</div>

The result surprised me. Following the model's preference is *worse* than a random order: the elite solutions already agree with the model, so the high-priority flips are exactly the ones the search has learned to reject, and the budget is spent on rejected moves. Uncertainty points the other way, at bits where a flip still has a real chance of being accepted. That distinction runs through the rest of the project.

### Crossover that switches itself off

Uniform crossover swaps each differing bit with probability 0.5, which late in a run tears apart structure the population has settled on. Entropy-restricted uniform crossover (ER-UXO) computes a confidence for each bit and swaps only where the parents differ *and* the confidence is below a threshold τ:

```java
// EntropyRestrictedUniformXO.java
double conf = 2.0 * Math.abs(p[i] - 0.5);
if (conf < tau && random.nextDouble() < 0.5) {
    problem.exchangeBits(child1Index, child2Index, i);
}
```

A sweep of nine τ values on a representative instance put the optimum at τ = 0.05, with performance drifting back toward plain uniform crossover as τ grew. Counting eligible bits over a run shows why:

<figure>
  <img src="{{ '/assets/images/projects/fyp/eruxo-eligible-bits.png' | relative_url }}" alt="Line chart of crossover-eligible bits per generation for ER-UXO at tau 0.05: 250 bits for the first five generations, falling to about 5 by generation 40 and staying there to generation 60">
  <figcaption>Crossover-eligible bits per generation at τ = 0.05 on a 250-variable MAX-SAT instance, median and mean over 31 runs. The gate is fully open for the first five generations and almost closed by generation 40.</figcaption>
</figure>

<div class="dotplot">
  <div class="dotplot__title">MAX-SAT crossover configurations &middot; average rank over 12 instances (1 = best)</div>
  <div class="dotplot__row dotplot__row--accent">
    <span class="dotplot__label">ER-UXO + entropy-guided local search</span>
    <span class="dotplot__track"><span class="dotplot__dot" style="left:29.3%"></span></span>
    <span class="dotplot__value">1.88</span>
  </div>
  <div class="dotplot__row">
    <span class="dotplot__label">ER-UXO + DBHC</span>
    <span class="dotplot__track"><span class="dotplot__dot" style="left:44%"></span></span>
    <span class="dotplot__value">2.32</span>
  </div>
  <div class="dotplot__row">
    <span class="dotplot__label">Uniform crossover + entropy-guided local search</span>
    <span class="dotplot__track"><span class="dotplot__dot" style="left:55.7%"></span></span>
    <span class="dotplot__value">2.67</span>
  </div>
  <div class="dotplot__row">
    <span class="dotplot__label">Uniform crossover + DBHC</span>
    <span class="dotplot__track"><span class="dotplot__dot" style="left:71%"></span></span>
    <span class="dotplot__value">3.13</span>
  </div>
  <div class="dotplot__axis">
    <span class="dotplot__label"></span>
    <span class="dotplot__ticks"><span style="left:0">1</span><span style="left:33.3%">2</span><span style="left:66.7%">3</span><span style="left:100%">4</span></span>
    <span class="dotplot__value"></span>
  </div>
</div>

So the value of the operator is not more mixing but *controlled* mixing: the model acts as a gate that is open early, when every bit is uncertain, and closes as the search stabilises.

### The learning loop

The probability vector starts at 0.5 everywhere and, after each generation's replacement, moves toward the best individual: `p[j] = (1 − α)·p[j] + α·elite[j]`, clipped to [0.05, 0.95] so no bit ever becomes fully deterministic. Two settings needed evidence rather than convention:

- **Learning rate.** A grid of eight α values and five clamp bounds (40 cells, 11 runs each) showed that α dominates and that α = 0.01 was best under every clamp setting. The heatmaps below show what the rate does to the model: at α = 0.01 the probabilities drift smoothly and keep their uncertainty for most of the run; at α = 0.10 they polarise within about 20 generations and the operators lose the signal they rely on.
- **Learning scope.** Updating toward the top-K individuals for K in {1, 3, 5, 10, 25} showed K = 1 to be the most consistent setting (average rank 2.17 against 3.08 or worse for every larger K). Blending several good solutions blurs exactly the per-bit signal the operators need.

<figure>
  <img src="{{ '/assets/images/projects/fyp/pv-heatmap-maxsat.png' | relative_url }}" alt="Two heatmaps of the PBIL probability vector over 60 generations, one bit per row, blue for probability 0 and red for probability 1. At learning rate 0.01 the colours stay pale for most of the run; at learning rate 0.10 rows turn saturated red or blue within about 20 generations">
  <figcaption>How fast the model commits: the probability that each of the 250 bits is 1 (blue 0, red 1) over 60 generations on the same MAX-SAT instance, at α = 0.01 (left) and α = 0.10 (right).</figcaption>
</figure>

### A comparison every algorithm can accept

Memetic algorithms spend many fitness evaluations per generation on local search, so comparing algorithms at a fixed number of generations quietly hands them a bigger budget. For the final comparison I measured the median total evaluations my framework used over 60 generations on one instance across 31 runs, which came to 755,678, and gave every competitor exactly that budget: a plain genetic algorithm, standalone PBIL, a memetic algorithm with DBHC, one with steepest-descent hill climbing, and the proposed PBIL-MA-Entropy. Rankings are paired: run *k* of each algorithm shares a seed lineage, and ranks are computed within each (instance, run) pair before averaging, so one lucky run cannot carry a method.

## Results

<div class="dotplot dotplot--dual">
  <div class="dotplot__title">Average rank under an equal evaluation budget &middot; the same five algorithms, 12 instances per domain, 31 runs each (1 = best)</div>
  <div class="dotplot__head">
    <span></span>
    <span></span>
    <span><i class="dotplot__key"></i>MAX-SAT</span>
    <span><i class="dotplot__key dotplot__key--hollow"></i>Knapsack</span>
  </div>
  <div class="dotplot__row dotplot__row--accent">
    <span class="dotplot__label">PBIL-MA-Entropy (proposed)</span>
    <span class="dotplot__track"><span class="dotplot__link" style="left:4.2%;width:57.3%"></span><span class="dotplot__dot" style="left:4.2%"></span><span class="dotplot__dot dotplot__dot--hollow" style="left:61.5%"></span></span>
    <span class="dotplot__value">1.17</span>
    <span class="dotplot__value dotplot__value--alt">3.46</span>
  </div>
  <div class="dotplot__row">
    <span class="dotplot__label">MA-DBHC</span>
    <span class="dotplot__track"><span class="dotplot__link" style="left:31.3%;width:32.2%"></span><span class="dotplot__dot" style="left:31.3%"></span><span class="dotplot__dot dotplot__dot--hollow" style="left:63.5%"></span></span>
    <span class="dotplot__value">2.25</span>
    <span class="dotplot__value dotplot__value--alt">3.54</span>
  </div>
  <div class="dotplot__row">
    <span class="dotplot__label">GA, no local search</span>
    <span class="dotplot__track"><span class="dotplot__link" style="left:35.4%;width:8.4%"></span><span class="dotplot__dot" style="left:43.8%"></span><span class="dotplot__dot dotplot__dot--hollow" style="left:35.4%"></span></span>
    <span class="dotplot__value">2.75</span>
    <span class="dotplot__value dotplot__value--alt">2.42</span>
  </div>
  <div class="dotplot__row">
    <span class="dotplot__label">Standalone PBIL</span>
    <span class="dotplot__track"><span class="dotplot__link" style="left:50%;width:22.9%"></span><span class="dotplot__dot" style="left:72.9%"></span><span class="dotplot__dot dotplot__dot--hollow" style="left:50%"></span></span>
    <span class="dotplot__value">3.92</span>
    <span class="dotplot__value dotplot__value--alt">3.00</span>
  </div>
  <div class="dotplot__row">
    <span class="dotplot__label">MA-SDHC</span>
    <span class="dotplot__track"><span class="dotplot__link" style="left:39.6%;width:58.3%"></span><span class="dotplot__dot" style="left:97.9%"></span><span class="dotplot__dot dotplot__dot--hollow" style="left:39.6%"></span></span>
    <span class="dotplot__value">4.92</span>
    <span class="dotplot__value dotplot__value--alt">2.58</span>
  </div>
  <div class="dotplot__axis">
    <span class="dotplot__label"></span>
    <span class="dotplot__ticks"><span style="left:0">1</span><span style="left:25%">2</span><span style="left:50%">3</span><span style="left:75%">4</span><span style="left:100%">5</span></span>
    <span class="dotplot__value"></span>
    <span class="dotplot__value"></span>
  </div>
</div>
<p class="dotplot__note">Budgets: 755,678 evaluations on MAX-SAT, 4,517,996 on Knapsack. On Knapsack the proposed configuration keeps entropy-guided local search but uses plain uniform crossover, because ER-UXO had already been shown not to transfer there (paired rank 1.56 against 1.44 for the uniform baseline).</p>

| Algorithm | MAX-SAT rank | MAX-SAT wins | Knapsack rank | Knapsack wins |
|---|---:|---:|---:|---:|
| **PBIL-MA-Entropy** (proposed) | **1.17** | **11** | 3.46 | 7 |
| MA-DBHC | 2.25 | 2 | 3.54 | 7 |
| GA | 2.75 | 1 | **2.42** | **11** |
| PBIL | 3.92 | 0 | 3.00 | 9 |
| MA-SDHC | 4.92 | 0 | 2.58 | 10 |

Wins count the best or tied-best median over 31 runs on each of the 12 instances, so on the small Knapsack instances several algorithms share a win.

<figure>
  <img src="{{ '/assets/images/projects/fyp/maxsat-instance0-convergence.png' | relative_url }}" alt="Convergence curves of median best-so-far unsatisfied clauses over 60 generations on MAX-SAT instance 0 for four crossover and local-search pairings. Uniform crossover with DBHC flattens near 60; ER-UXO with entropy-guided local search reaches single digits by generation 35">
  <figcaption>Median best-so-far objective (unsatisfied clauses, lower is better) over 60 generations on MAX-SAT instance 0, 31 runs. Uniform crossover with DBHC settles near 60; the proposed pairing of ER-UXO with entropy-guided local search reaches single digits by generation 35.</figcaption>
</figure>

- **MAX-SAT.** In a direct paired comparison against the unguided memetic baseline across all 12 instances, the full configuration had the better average paired rank (1.31 against 1.69), ahead on 10 instances, level on one and behind on one. Under the shared budget it then won or tied 11 of 12 instances against four other algorithms, so the gain is not an artefact of extra evaluations.
- **0–1 Knapsack.** Entropy-guided local search stayed competitive (paired rank 1.99 against 1.80 for DBHC, well ahead of the conflict-guided variant at 3.24), but crossover gating hurt, and under the shared budget a GA with no local search at all won. The capacity constraint already tells the search which bits matter, so the model's uncertainty stops being informative, and closing crossover early removes diversity that the constrained landscape needs.

## What I learned

- **Encode uncertainty, not preference.** The same probability vector was harmful when it enforced the model's belief and helpful when it pointed at what the model had not settled. A guidance signal is only as good as the quantity it measures.
- **Fairness changes conclusions.** Under equal budgets a GA with no local search finished third on MAX-SAT and first on Knapsack, and steepest descent went from plausible to last. I now treat the budget definition as part of the experimental design, not a footnote.
- **A negative result with a mechanism is still a result.** The Knapsack transfer failed in an explainable way, and that explanation is what I would carry into a new domain: adapt τ to how active the constraints are, or learn a feasibility-aware model, before expecting the same operators to work.
- **Build the harness before the experiments.** The Config + Runner pattern, paired seeds and the CSV-to-matplotlib pipeline meant that each of the 44 experiment classes and 61 plotting scripts was a small addition rather than a rewrite, and every figure in the dissertation can be regenerated from a single command.

I did this project in the Computational Optimisation Lab at the University of Nottingham. The code builds on the department's course search framework and was submitted for assessment, so it is not public. The dissertation linked above contains the full method, every result table and the algorithms in pseudocode, and I am glad to walk through any part of the implementation in conversation.
