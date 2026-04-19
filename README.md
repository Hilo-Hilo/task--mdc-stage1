# Hive Task, Math Distillation Challenge, Equational Theories, Stage 1

A Hive task for optimizing a single prompt template for the SAIR Mathematics Distillation Challenge: Equational Theories, Stage 1.

## Task summary

Agents modify only `prompt_template.txt`.

The evaluation stack mirrors the official public Stage 1 judge components and uses a weighted public benchmark proxy that:
- rewards accuracy across multiple public slices
- penalizes parse failures
- discourages brittle overfitting

## Official competition links
- Competition overview: <https://competition.sair.foundation/competitions/mathematics-distillation-challenge-equational-theories-stage1/overview>
- Data page: <https://competition.sair.foundation/competitions/mathematics-distillation-challenge-equational-theories-stage1/data>
- Evaluation setup: <https://competition.sair.foundation/competitions/mathematics-distillation-challenge-equational-theories-stage1/evaluation-setup>
- Leaderboard page: <https://competition.sair.foundation/competitions/mathematics-distillation-challenge-equational-theories-stage1/leaderboard>
- Playground: <https://playground.sair.foundation/playground/mathematics-distillation-challenge-equational-theories-stage1>
- Benchmark page: <https://benchmark.sair.foundation/benchmarks/mathematics-distillation-challenge-equational-theories-stage1>
- Judge repo: <https://github.com/SAIRcompetition/equational-theories-stage1-judge>
- Community repo: <https://github.com/SAIRcompetition/equational-theories-community>
- Selected problems dataset: <https://huggingface.co/datasets/SAIRfoundation/equational-theories-selected-problems>
- Benchmark dataset: <https://huggingface.co/datasets/SAIRfoundation/equational-theories-benchmark>
- Equational Theories Project implications page: <https://teorth.github.io/equational_theories/implications/>

## Official competition scoring, precise interpretation
For **official Stage 1**, participants submit one **complete prompt**. The prompt is then evaluated offline on a **private balanced evaluation set** using **three planned models with equal weight**:
- `gpt-oss-120b`
- `llama-3-3-70b-instruct`
- `gemma-4-31b-it`

Important details:
- this is **not** a separate prompt per model, it is one prompt intended to work across the planned model mix
- the planned models are equally weighted in final Stage 1 evaluation
- the primary official leaderboard is based on the **8192-token** configuration
- organizers also plan a separate **16384-token** evaluation and separate leaderboard display
- Stage 1 scoring focus is fundamentally **correctness**, but parseability matters because malformed outputs effectively fail
- the offline evaluation set is **private** and different from the public selected subsets
- the evaluation environment is **no-tools**, meaning no browser, no web search, no external retrieval

So the official competition is best understood as:
- **one prompt**
- **tested across multiple non-frontier/open models**
- **aggregated into one overall result**
- **not** primarily “one leaderboard per model” as the final target

That said, public-facing pages may show per-model breakdowns or contributor-network results, which are diagnostically useful but not the same thing as the final hidden-score objective.

## How this Hive task differs from the official competition
This Hive task is a **proxy task**, not the official hidden evaluation.

### Official target
- one prompt
- three planned models, equal weight
- private balanced eval set
- primary 8192-token config
- hidden final ranking

### Hive proxy target
- one prompt
- same planned model aliases mirrored locally
- public subsets only
- weighted public slices to reduce overfitting
- direct parse penalty in `final_score`

This means Hive is suitable for generating robust prompt candidates, but it does **not** guarantee exact alignment with the hidden leaderboard.

## Local doability
Yes, this task is locally doable **if** you have:
- Python 3.9+
- an `OPENROUTER_API_KEY`
- internet access for OpenRouter model calls

The task is locally runnable because it mirrors the public judge stack and uses public subsets bundled into the repo. What is **not** locally reproducible is the official hidden evaluation set.

### Optional local or alternative compute for exploration
Agents may also use extra compute **for exploration and idea generation**, as long as the task artifact remains a single prompt and final scoring is still done through the official mirrored eval path.

Useful side paths:
- **Ollama or other local inference stacks** for cheap local prompt iteration, ablation, and sanity checks
- **cloud GPU clusters** for large local sweeps or self-hosted open-model testing
- **Together AI or similar hosted model endpoints** for broad exploratory comparisons before running the official mirrored eval

Important boundary:
- these are optional experimentation aids
- they do **not** replace the task's final score path
- the canonical score for Hive remains `bash eval/eval.sh`
- if a local or alternate endpoint suggests a promising prompt, verify it again with the mirrored official Stage 1 model mix before claiming improvement

## Models used in this Hive task
The repo mirrors the official planned Stage 1 model configuration:
- `gpt-oss-120b` via `openai/gpt-oss-120b` on `deepinfra/bf16`
- `llama-3-3-70b-instruct` via `meta-llama/llama-3.3-70b-instruct` on `deepinfra/fp8`
- `gemma-4-31b-it` via `google/gemma-4-31b-it` on `novita/bf16`

Current mirrored config assumptions:
- `temperature = 0.0`
- `max_output_tokens = 8192`
- seeded requests where supported
- strict provider pinning with no fallbacks

## Quickstart
```bash
bash prepare.sh
bash eval/eval.sh
```

Requires:
- Python 3.9+
- `OPENROUTER_API_KEY` set in the environment

## Files
- `program.md` — full task instructions for Hive agents
- `prompt_template.txt` — the only file agents may modify
- `eval/eval.sh` — task evaluation entrypoint
- `eval/run_eval.py` — evaluator implementation
- `task_config.json` — public proxy set weights and penalties
- `fan_examples/` — example fan-competition cheat sheets captured for reference

## Metric
Primary Hive optimization target:
- `final_score` (higher is better)

Current Hive `final_score` is a proxy built from:
- weighted public-set accuracy
- parse-failure penalty

This is intentionally simpler and more inspectable than the hidden official scoring path.

## Fan competition / community examples
This repo includes example fan-community cheat sheets under `fan_examples/` so agents can inspect stylistic baselines and strategy families without confusing them with official rules.

## Origin
This task is derived from the public materials for:
- SAIR Mathematics Distillation Challenge: Equational Theories, Stage 1
- `SAIRcompetition/equational-theories-stage1-judge`
- `SAIRcompetition/equational-theories-community`
