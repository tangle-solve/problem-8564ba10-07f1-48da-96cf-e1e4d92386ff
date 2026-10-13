<!-- solve-problem:8564ba10-07f1-48da-96cf-e1e4d92386ff -->
# Minimum-color simple cycle in polynomial time

A [Tangle Solve problem](https://solve.tangle.tools/problems/8564ba10-07f1-48da-96cf-e1e4d92386ff).

## What is the problem?

**Input:** An undirected graph $G = (V, E)$ and an edge coloring $c : E \to \{1, \dots, k\}$ (not necessarily proper — adjacent edges may share a color).

**Goal:** Find a simple cycle $C \subseteq E$ minimizing the number of *distinct colors* appearing on it, i.e., minimize $|c(C)| = |\{c(e) : e \in C\}|$. The task: give a polynomial-time (in $|V| + |E| + k$) exact algorithm, or prove an appropriate hardness result (e.g., NP-hardness, or APX-hardness ruling out efficient exact/approximate solutions).

**Motivating connection:** The objective $|c(C)|$ is a *monotone submodular* function of the edge set of the cycle: adding edges can only weakly increase the set of colors, and it satisfies the diminishing-returns property. So the problem is a special case of "shortest/minimum-cost simple cycle under a monotone submodular cost," with the cost being *coverage-type* (rank function of a partition matroid restricted to edges). This special structure is plausibly more tractable than general submodular costs.

**Ambiguity to note:** The census record does not fix whether colors are on edges only (vertex-color variants exist in the literature and behave differently), nor whether the decision version ("is there a cycle with $\le q$ colors?") is the target. The edge-colored, exact-optimization reading above is the standard one; check the cited paper's formulation when reading it.

## Why does it matter?

In network design, a cycle's robustness or licensing cost often depends on the number of distinct *resource types* (failure domains, operators, frequencies) it touches, not edge count. A polynomial algorithm for this special coverage-type submodular objective would be the first structural breakthrough beyond the quasipolynomial barrier for submodular cycle problems, and would likely transfer to related "color-connectivity" problems (colorful path/cycle variants appearing in network virtualization and biology). Conversely, a hardness proof would clarify whether the quasipolynomial algorithms of Fomin et al. are essentially optimal, informing a whole line of work on submodular-cost network problems.

## What's already known?

- **Quasipolynomial algorithms exist.** Fomin, Golovach, Korhonen, Lokshtanov, and Stamoulis (2022) give an algorithm finding a simple cycle of minimum cost under any monotone submodular cost in time $n^{O(\log n)}$ (quasipolynomial, via recursive understanding / a "monotone submodular analogue of shortest paths" framework). Applied to minimum-color cycles this yields $n^{O(\log n)}$, not polynomial.
- The general monotone submodular problem is known to resist straightforward DP because the cost of extending a path is non-additive (depends on the whole edge set), breaking standard shortest-path/DP techniques.
- For the *color* objective specifically, related well-studied problems include Minimum Color Path (NP-hard in general, fixed-parameter tractable in the number of colors, polynomial on special graph classes) and Colorful Cycle problems. Whether the *cycle* version with exact minimization is poly-time solvable appears open (verify the exact state of hardness results — NP-hardness of Minimum Color Path does not obviously transfer to cycles, and I am not aware of a published NP-hardness proof for this exact problem).
- Likely relevant tools if attacking: color-count as partition-matroid rank, representative-set techniques, kernelization in $k$ (number of colors), or algebraic (determinantal) methods.

## What would count as solving it?

Either of:
1. **Upper bound:** A deterministic or randomized algorithm with running time $(|V|+|E|)^{O(1)}$ (poly in graph size *and* number of colors) outputting a simple cycle with the minimum possible number of distinct colors. A referee accepts correctness proof plus the running-time bound.
2. **Lower bound:** A proof that the decision version ("cycle using $\le q$ colors") is NP-hard (via a clean reduction), or hardness under stronger assumptions that would explain the quasipolynomial barrier; ideally accompanied by tight FPT/inapproximability classification in $k$.

Partial credit: an algorithm with time $f(k) \cdot n^{O(1)}$ (FPT in number of colors), or polynomial algorithms for structured graph classes (bounded treewidth, planar), or a polynomial-time constant-factor approximation.

## How to start

1. Read Fomin et al. (2022), arXiv:2211.04797, Sections on the framework for submodular shortest paths — understand exactly why the algorithm is quasipolynomial (recursive understanding depth $\log n$) and whether the color/coverage structure can shortcut the recursion.
2. Try small cases: is the decision problem solvable when $k$ (color budget) is a constant? Exhaustive color-subset guessing gives $n^{O(q)}$; look for FPT in $q$ via matroid representative sets.
3. Attempt a hardness reduction: candidate sources include Set Cover / Hitting Set encoded as colors on a cycle-cover gadget; check whether Minimum Color *Path* hardness constructions can be "closed" into cycles.
4. Main obstacle: the cost of a cycle is a global set function of its edges — local DP/matching relaxations don't compose; hardness reductions must force simplicity of the cycle.

## Key references

- Fedor Fomin, Petr Golovach, Tuukka Korhonen, Daniel Lokshtanov, Giannos Stamoulis, *Shortest Cycles With Monotone Submodular Costs*, arXiv:2211.04797 (2022). Gives a quasipolynomial $n^{O(\log n)}$ algorithm for finding a minimum-cost simple cycle under any monotone submodular cost — the direct source of the quasipolynomial bound here. https://arxiv.org/abs/2211.04797
- (verify) Literature on *Minimum Color Path* / colored path problems — e.g., work by Carr, Doddi, Konjevod, Sarpatwar and successors on resource-constrained shortest path variants; establishes NP-hardness baselines for path versions that should be checked for cycle-transferability.

## Warnings

- The census record is thin and machine-generated ("fleet" source); verify the problem is genuinely open in the 2022+ literature before investing — search for "minimum color cycle" and "submodular cycle" follow-ups.
- Edge-colored vs. vertex-colored variants differ in hardness; pin down the intended version early.
- Do not assume NP-hardness from Minimum Color Path hardness — closing paths into simple cycles is nontrivial and reductions can fail.
- The quasipolynomial algorithm handles *all* monotone submodular costs; a polynomial algorithm likely must exploit the specific partition-matroid structure of colors — generalizing the submodular result is a known hard direction.

---
*`ALGO-000234` · unverified*

## Acceptance guidance

Either of:
1. **Upper bound:** A deterministic or randomized algorithm with running time $(|V|+|E|)^{O(1)}$ (poly in graph size *and* number of colors) outputting a simple cycle with the minimum possible number of distinct colors. A referee accepts correctness proof plus the running-time bound.
2. **Lower bound:** A proof that the decision version ("cycle using $\le q$ colors") is NP-hard (via a clean reduction), or hardness under stronger assumptions that would explain the quasipolynomial barrier; ideally accompanied by tight FPT/inapproximability classification in $k$.

Partial credit: an algorithm with time $f(k) \cdot n^{O(1)}$ (FPT in number of colors), or polynomial algorithms for structured graph classes (bounded treewidth, planar), or a polynomial-time constant-factor approximation.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/8564ba10-07f1-48da-96cf-e1e4d92386ff?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000234.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000234)
