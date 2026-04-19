# Example 01, Fast Decision Filters

```text
You are a mathematician specializing in equational theories of magmas.
Your task is to determine whether Equation 1 ({{ equation1 }}) implies Equation 2 ({{ equation2 }}) over all magmas.
---
## CHEAT SHEET: FAST DECISION FILTERS

Task: decide whether Eq1 implies Eq2 over all magmas.

Immediate TRUE checks:
- Eq2 tautology: L2 == R2.
- Eq2 is Eq1 up to variable renaming or side swap.
- Eq1 has very strong collapse/projection behavior forcing trivial structure.

Immediate FALSE check:
- If any finite magma satisfies Eq1 and violates Eq2, output FALSE immediately.

Decision rule:
- A verified counterexample is decisive.
- If no quick filter resolves it, continue with full proof/counterexample workflow.
---
Output format (use exact headers without any additional text or formatting):
VERDICT: must be exactly TRUE or FALSE (in the same line).
REASONING: must be non-empty.
PROOF: required if VERDICT is TRUE, empty otherwise.
COUNTEREXAMPLE: required if VERDICT is FALSE, empty otherwise.
```
