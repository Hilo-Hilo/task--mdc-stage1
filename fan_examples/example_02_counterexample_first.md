# Example 02, Counterexample First

```text
You are a mathematician specializing in equational theories of magmas.
Your task is to determine whether Equation 1 ({{ equation1 }}) implies Equation 2 ({{ equation2 }}) over all magmas.
---
## CHEAT SHEET: EQUATIONAL THEORIES IMPLICATION
### 1. Objective
- Given Eq1 (`L1 = R1`) and Eq2 (`L2 = R2`), decide if Eq1 implies Eq2 over all magmas.
- Strategy here:
 - Try to falsify implication quickly.
 - If falsification fails in strong search space, then attempt proof.

---
### 2. Core Principle
- A single verified finite counterexample is decisive.
- Therefore prioritize search for a magma where:
 - Eq1 holds universally,
 - Eq2 fails for at least one assignment.

---
### 3. Fast Structural Filters Before Search
- Canonicalize variables and normalize equation forms.
- Check quick positives:
 - Eq2 tautology (`L2 == R2`),
 - Eq2 alpha-equivalent to Eq1.
- If not resolved, proceed immediately to model search.

---
### 4. Counterexample Search Ladder
Run in this order:

1. Size-2 exhaustive
- Enumerate all binary tables on domain {0,1}.
- Evaluate Eq1 and Eq2 exactly.

2. Size-3 targeted families
- left-zero / right-zero / constant operations,
- projection-like operations,
- commutative table subsets,
- non-idempotent and asymmetric constructions.

3. Algebraic parametric families
- modular-linear forms, e.g. x*y = a x + b y + c (mod n).
- scan small primes/moduli.

4. Boolean-like operators
- xor / and / or / implication-like tables.

5. Random + local improvement
- random tables with hill-climb mutation to satisfy Eq1 first;
- once Eq1 is near-satisfied, optimize for Eq2 violation.

Stop immediately when a verified counterexample appears.

---
### 5. Validation Protocol (Non-Negotiable)
For any candidate table:

1. Verify Eq1 on all assignments of Eq1 variables.
2. If Eq1 passes, check Eq2 on all assignments (or find one failing witness).
3. Report:
- domain,
- full operation table,
- one explicit failing assignment for Eq2,
- value trace of both sides on that assignment.

Never output FALSE without this full verification.

---
### 6. Heuristic Prioritization Signals
Use these to rank which candidates to try first:
- lower Eq1 complexity often easier to satisfy randomly;
- low subterm overlap from Eq1 to Eq2 suggests likely non-implication;
- Eq2 structurally stricter than Eq1 suggests higher falsification chance;
- asymmetric variable occurrences often produce easy violating assignments.

These signals guide search order only.

---
### 7. Counterexample-Friendly Candidate Templates
Useful quick templates:
- constant row/column tables,
- left projection (`x*y=x`) variants,
- right projection (`x*y=y`) variants,
- near-projection with one altered cell,
- affine/modular tables with small modulus.

Template mutation trick:
- keep cells preserving Eq1 behavior;
- perturb cells frequently touched by Eq2’s distinctive subterms.
```
