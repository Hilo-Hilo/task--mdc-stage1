# Math Distillation Challenge, Stage 1 Prompt Optimization

Improve a single Stage 1 prompt template for the SAIR Mathematics Distillation Challenge: Equational Theories. Agents optimize `prompt_template.txt`, and the task is evaluated by running the mirrored public judge stack on public problem sets with a weighted score that rewards accuracy and penalizes parse failures.

## Setup

1. **Read the in-scope files**:
   - `prompt_template.txt` — the main artifact. You modify this.
   - `task_config.json` — scoring weights and subset definitions for the Hive proxy. Read-only.
   - `README.md` — contains the official competition links, official scoring interpretation, local-doability notes, and model details.
   - `eval/eval.sh` — runs evaluation and prints the official Hive score block. Do not modify.
   - `eval/run_eval.py` — evaluation runner. Do not modify.
   - `evaluation_models.json` — mirrored planned Stage 1 model config from the official judge repo. Do not modify.
   - `judge.py`, `llm.py`, `models.py`, `prompt.py` — mirrored Stage 1 helper modules from the judge repo. Do not modify.
   - `fan_examples/` — example fan/community cheat sheets for reference. Do not treat them as official rules.
2. **Run prepare**: `bash prepare.sh` to create a local virtualenv and install dependencies.
3. **Verify data exists**: check that `data/` contains `normal.jsonl`, `hard1.jsonl`, and `hard2.jsonl`.
4. **Initialize results.tsv**: create `results.tsv` with just the header row if it does not exist.
5. **Run baseline**: `bash eval/eval.sh` to establish the starting score.

## The official competition, precise definition
The official SAIR Stage 1 competition evaluates **one complete prompt** against a **private balanced evaluation set** using **three planned models with equal weight**:
- `gpt-oss-120b`
- `llama-3-3-70b-instruct`
- `gemma-4-31b-it`

The primary official leaderboard uses the **8192-token** configuration. Organizers also plan a separate **16384-token** evaluation and separate leaderboard display.

Interpret this correctly:
- this is one prompt optimized to work across the model mix
- not one separate winning prompt per model
- correctness is the core scoring target
- parseability matters because malformed outputs fail practical evaluation
- the hidden offline set differs from the public selected subsets
- the evaluation environment is no-tools

## The Hive benchmark
This Hive task is a **proxy** for the official competition.

It uses:
- public selected problem subsets only
- the mirrored official planned model aliases
- a weighted public aggregate instead of the hidden offline set
- an explicit parse penalty to discourage brittle prompt hacks

This task is intentionally conservative about overfitting. The score is a weighted aggregate across several public slices rather than a single public board.

## Local and alternate-compute guidance
The canonical evaluation path is still the mirrored official eval in this repo. However, agents may use additional compute for exploration before validating improvements.

Allowed exploration lanes:
- local inference stacks such as **Ollama**
- self-hosted open-model serving on local or lab GPUs
- hosted GPU/model endpoints such as **Together AI**
- private clusters for prompt sweeps or ablations

Rules for using them:
- treat them as auxiliary search tools only
- do not replace the final scoring path with alternate endpoints
- always re-check promising prompts using `bash eval/eval.sh`
- never claim a Hive score improvement from a non-canonical endpoint alone

## Experimentation

**What you CAN do:**
- Modify `prompt_template.txt` only.
- Change instructions, reasoning scaffolds, compression strategy, proof-vs-counterexample guidance, ordering, formatting emphasis, and heuristics.
- Optimize for robustness across the official planned model mix, not just one model.
- Use the fan/community examples for inspiration, but treat them only as baselines or idea sources.

**What you CANNOT do:**
- Do not modify `eval/`, `prepare.sh`, `task_config.json`, `evaluation_models.json`, `judge.py`, `llm.py`, `models.py`, `prompt.py`, `README.md`, `fan_examples/`, or any data files.
- Do not add external retrieval, tool use, or internet assumptions into the prompt.
- Do not hardcode answers to specific problem IDs or build prompt logic around dataset position.
- Do not create multi-file prompt systems. The artifact is one prompt file.

**The goal: maximize `final_score`.** Higher is better.

`final_score` is defined in this Hive task as:
- weighted public-set accuracy
- minus parse-failure penalty

The weighted public accuracy is aggregated across:
- `normal`
- `hard1`
- `hard2`
- `hard3_holdout` proxy slice

`hard3_holdout` is approximated here by a held-out tail slice from `hard2`. This is not an official SAIR split. It exists only to reduce single-slice overfitting pressure in Hive.

**Simplicity criterion**: all else equal, prefer shorter, cleaner, more interpretable prompt structures over baroque prompt hacks.

## Strategic guidance
Useful directions include:
- compact algebraic rewrite heuristics
- explicit proof-vs-counterexample branching
- output-discipline improvements for parser stability
- model-agnostic prompt organization
- anti-overfitting prompt structures that generalize across subsets

Avoid shallow leaderboard chasing on one slice. A prompt that spikes one public slice while harming transfer or parse reliability is not a good Hive outcome.

## Output format
`bash eval/eval.sh` must end with:

```text
---
final_score:      0.123456
weighted_acc:     0.234567
parse_rate:       0.980000
correct:          123
total:            456
```

Agents should extract score with:

```bash
grep '^final_score:' run.log
```
