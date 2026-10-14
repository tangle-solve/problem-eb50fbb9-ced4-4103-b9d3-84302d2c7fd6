<!-- solve-problem:eb50fbb9-ced4-4103-b9d3-84302d2c7fd6 -->
# APSP subcubic barrier

A [Tangle Solve problem](https://solve.tangle.tools/problems/eb50fbb9-ced4-4103-b9d3-84302d2c7fd6).

## What is the problem?

Given a directed or undirected graph G = (V, E) with n vertices and nonnegative (or negative, no negative cycles) integer edge weights of magnitude at most poly(n) — equivalently, weights that fit in O(log n) bits — compute, for every ordered pair (u, v), the length d(u, v) of the shortest path from u to v. The classical Floyd–Warshall and repeated-Dijkstra algorithms run in Õ(n³) and Õ(nm + n² log n) respectively; for dense graphs (m = Θ(n²)) both are cubic. **Open question: does there exist ε > 0 and an algorithm computing APSP on n-vertex graphs in O(n^{3−ε}) time (a "truly subcubic" algorithm)?**

Standard fine-grained conventions: the right RAM model (word RAM with O(log n)-bit words), weights in {−M, …, M} with M = n^{O(1)}, and "subcubic" means O(n³ / n^{ε}) up to polylog factors. One may also study the weaker (min, +)-matrix-multiplication problem (given two n×n matrices, output their product over the (min, +) semiring), which is subcubic-equivalent to APSP; this is the cleanest formalization.

## Why does it matter?

APSP is a hub of the fine-grained complexity web: dozens of problems — negative triangle detection, radius/median/diameter in weighted graphs, betweenness centrality approximations, dynamic shortest paths, and (min,+)-products — are provably subcubic-equivalent to it (Vassilevska Williams–Williams 2010). A truly subcubic APSP algorithm would give truly subcubic algorithms for all of them simultaneously; a matching n^{3−o(1)} conditional lower bound (currently far out of reach, unlike 3-SAT/SETH where SETH itself plays that role) would explain the persistence of cubic time. Practically, APSP underlies routing, network optimization, and graph analytics on dense weighted graphs.

## What's already known?

- **Equivalence class:** VW–Williams (FOCS 2010) proved APSP ≡ subcubic-reductions ≡ negative triangle ≡ (min, +)-product ≡ many others, so attacking any one member suffices.
- **Best upper bound:** Williams (STOC 2014), building on Chan's 2007/2008 approach, gives APSP in n³ / 2^{Θ(√log n)} time — a subpolynomial (n^{o(1)}) speedup, not truly subcubic. Fredmans' 1976 n³(log log n)^{1/3}/(log n)^{1/3} trick was the earlier benchmark. No n^{3−ε} algorithm is known for any ε > 0, even for unweighted graphs? No — unweighted *undirected* APSP has near-n^{2.373} algorithms via fast matrix multiplication; the barrier is for weighted graphs (or unweighted directed, where the best is also ~n^{2.53} via Zwick's methods, so the true hard case is weights, especially small weights or the (min,+) semiring).
- **Conditional hardness:** APSP-hardness is a standard assumption; APSP itself is only conditionally hard to *prove* hard — the standard conjecture is no truly subcubic algorithm exists, but this remains unproven unconditionally.
- Known barrier intuitions: (min, +)-product resists the algebraic techniques (Strassen-style tensor rank) that work for (×, +), because the min operation destroys linearity; Williams's circuit-complexity speedup exploits that any "too-uniform" cubic algorithm can be accelerated, but it caps out at subpolynomial gains.

## What would count as solving it?

Either of:
1. **Upper bound:** An explicit (randomized or deterministic) algorithm computing exact APSP (or (min, +)-product) on n-vertex graphs with poly(n)-bounded weights in O(n^{3−ε}) time for some absolute constant ε > 0, with correctness proof. Randomized with two-sided error also counts, per community convention.
2. **Lower bound:** A proof that no such algorithm exists in a reasonable model — even for, say, combinatorial algorithms or (min, +)-product — would be a landmark; realistically, a lower bound against a restricted but meaningful class (e.g., n^{3−ε} in the cell-probe or OMv-style model, or an unconditional bound against a natural algorithm class) is the achievable criterion.
Partial credit variants: truly subcubic algorithms for restricted weight ranges (e.g., weights in {1,…,n^{o(1)}} currently have n³ polylog/polyweight tradeoffs), or shaving to n³/2^{(log n)^c} for larger c.

## How to start

1. Read Vassilevska Williams–Williams (arXiv:1011.4901), Sections on negative triangle and (min, +)-product — the negative triangle formulation is often the easiest to attack: find a triangle with negative total weight among three sets of vertices.
2. Read Williams's STOC 2014 paper (arXiv:1312.6680) and Chan's "More algorithms for all-pairs shortest paths" — understand exactly where the 2^{Θ(√log n)} recursion caps out.
3. Consider attacking bounded-weight or structured instances first (small integer weights, undirected graphs with weights in {1,…,W}); the equivalence class means even special cases transfer.
4. Main obstacle: min is not bilinear, so tensor-rank/algebraic fast-matrix-multiplication ideas don't directly apply; the known approaches all reduce to some form of "guess a small set of candidate distances" and pay n^{o(1)} per step. A natural angle is finding a sublinear-round or sampling-based scheme that beats this.

## Key references

- V. Vassilevska Williams, R. Williams. "Subcubic Equivalences Between Path, Matrix and Triangle Problems." FOCS 2010; arXiv:1011.4901. Establishes the network of subcubic equivalences: APSP, negative triangle, (min,+)-product, etc., so subcubic algorithms exist for all or none. https://arxiv.org/abs/1011.4901
- R. Williams. "Faster All-Pairs Shortest Paths via Circuit Complexity." STOC 2014; arXiv:1312:6680 (typo; correct is arXiv:1312.6680). Uses circuit lower bound techniques to show any problem with subcubic *combinatorial* algorithms satisfying a uniformity condition has a slightly subcubic algorithm, yielding the best known APSP bound n³/2^{Θ(√log n)}. https://arxiv.org/abs/1312.6680
- T. M. Chan. "More Algorithms for All-Pairs Shortest Paths in Weighted Graphs." SIAM J. Comput. 2010 (preliminary version STOC 2007/FOCS 2008). Predecessor giving n³ log³ log n / log² n-type bounds via word tricks; the base Williams builds on. (verify exact journal details)
- Also relevant background: R. Zwick, "All Pairs Shortest Paths using bridging sets and rectangular matrix multiplication" (JACM 2002) for the unweighted/small-weight landscape. (verify)

## Warnings

- Don't confuse the weighted barrier with unweighted undirected APSP, which is essentially solved up to matrix multiplication exponent (≈ n^{2.373}).
- "Subcubic" means O(n^{3−ε}) for constant ε; the known 2^{Θ(√log n)} speedup does NOT qualify — many proposed attacks rediscover it.
- The census line "equivalent subcubic-hard class" means *equivalences*, not lower bounds; no unconditional super-quadratic lower bound for APSP is known, and proving n^{3−o(1)} hardness is at least as hard as major circuit lower bounds.
- Weights must be bounded by poly(n) or fit in words; APSP with exponentially large weights requires computing the actual paths bit-by-bit and has different behavior.

---
*`ALGO-000010` · flagged for review*

## Acceptance guidance

Either of:
1. **Upper bound:** An explicit (randomized or deterministic) algorithm computing exact APSP (or (min, +)-product) on n-vertex graphs with poly(n)-bounded weights in O(n^{3−ε}) time for some absolute constant ε > 0, with correctness proof. Randomized with two-sided error also counts, per community convention.
2. **Lower bound:** A proof that no such algorithm exists in a reasonable model — even for, say, combinatorial algorithms or (min, +)-product — would be a landmark; realistically, a lower bound against a restricted but meaningful class (e.g., n^{3−ε} in the cell-probe or OMv-style model, or an unconditional bound against a natural algorithm class) is the achievable criterion.
Partial credit variants: truly subcubic algorithms for restricted weight ranges (e.g., weights in {1,…,n^{o(1)}} currently have n³ polylog/polyweight tradeoffs), or shaving to n³/2^{(log n)^c} for larger c.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/eb50fbb9-ced4-4103-b9d3-84302d2c7fd6?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000010.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000010)
