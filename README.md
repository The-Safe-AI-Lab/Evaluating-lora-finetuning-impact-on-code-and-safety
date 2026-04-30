# Evaluating LoRA Finetuning Impact on Code and Safety

This repository contains experiment notebooks and result artifacts for studying how LoRA finetuning affects coding capability and safety behavior across different model settings.

## Project Snapshot

Current repository focus:

- MBPP-related evaluation and analysis
- Safety evaluation under multiple model sizes/checkpoints
- Aggregated run outputs and intermediate experiment results

## Repository Structure

Main notebooks:

- `fine_tuning.ipynb`: LoRA finetuning workflow and training steps
- `improved_training_colab.ipynb`: improved/iterative training setup (Colab-friendly)
- `mbpp_analysis_pipeline.ipynb`: MBPP analysis pipeline notebook
- `mbpp_improved_eval.ipynb`: refined MBPP evaluation workflow
- `safety_1b-pt.ipynb`: safety experiments for 1B-scale setup
- `safety_270m.ipynb`: safety experiments for 270M-scale setup
- `safety_4b-pt.ipynb`: safety experiments for 4B-scale setup

Result/artifact directories:

- `mbpp_results`
- `mbpp_pipeline_results`
- `mbpp_ckpt_results`
- `Connor's mbpp result`
- `safty-checkpoint-result`
- `sorry-bench-202503`

## Typical Workflow

1. Run finetuning notebooks to produce model checkpoints.
2. Run MBPP evaluation notebooks for code capability comparison.
3. Run safety notebooks for harm/safety behavior inspection.
4. Export and organize results into the corresponding output folders.
5. Use analysis notebooks to summarize trends across checkpoints and model scales.

## Detailed Usage Guide

### Step 0: Environment Setup

1. Create and activate a Python environment.
2. Install core dependencies used by notebooks.
3. Launch Jupyter and open notebooks from project root.

Example setup:

```bash
python -m venv .venv
source .venv/bin/activate  # Windows PowerShell: .venv\Scripts\Activate.ps1
pip install -U pip
pip install torch transformers datasets peft accelerate pandas numpy evaluate jupyter
jupyter lab
```

### Step 1: Run Finetuning

Use one of:

- `fine_tuning.ipynb` (main training flow)
- `improved_training_colab.ipynb` (Colab-oriented variant)

Recommended run-time settings to log for every run:

- base model name
- LoRA config (`r`, `alpha`, target modules, dropout)
- training split and sample size
- epochs, learning rate, batch size, seed
- output checkpoint path

Output expectation:

- one or more finetuned checkpoints saved under your configured output directory

### Step 2: Run MBPP Capability Evaluation

Primary notebooks:

- `mbpp_improved_eval.ipynb`: execute MBPP evaluation against selected checkpoint(s)
- `mbpp_analysis_pipeline.ipynb`: aggregate, compare, and visualize MBPP results

Suggested process:

1. Evaluate baseline model first (no finetuning).
2. Evaluate each finetuned checkpoint with the same MBPP split/settings.
3. Save raw predictions and pass/fail metrics per run.
4. Export summary tables into `mbpp_results` / `mbpp_pipeline_results`.

### Step 3: Run Safety Evaluation

Safety notebooks by scale/setup:

- `safety_270m.ipynb`
- `safety_1b-pt.ipynb`
- `safety_4b-pt.ipynb`

For consistent comparison:

- keep prompt set fixed across runs
- keep decoding config fixed (temperature, top_p, max_new_tokens)
- record model checkpoint id for each output file

Write outputs to safety-focused folders such as:

- `safty-checkpoint-result`
- `sorry-bench-202503`

### Step 4: Organize Artifacts

A practical per-run artifact bundle should include:

- config snapshot (or notebook parameter cell dump)
- model/checkpoint identifier
- generated outputs (JSON/CSV/TXT as available)
- aggregated metrics
- timestamp and random seed

Recommended naming pattern:

- `<task>__<model_or_ckpt>__<split_or_setting>__<yyyymmdd>`

### Step 5: Compare and Report

Use `mbpp_analysis_pipeline.ipynb` as the final comparison layer to:

- contrast baseline vs LoRA checkpoints
- inspect capability/safety trade-offs across model sizes
- produce shareable result summaries for team discussion

## Quick Reproduction Path (Minimal)

If you only want a fast end-to-end sanity run:

1. Run a short finetuning job in `fine_tuning.ipynb` (small sample/subset).
2. Evaluate that checkpoint in `mbpp_improved_eval.ipynb`.
3. Run one safety notebook (choose one scale).
4. Open `mbpp_analysis_pipeline.ipynb` and generate a small comparison table.
5. Save all outputs into one dated folder for traceability.

## Recommended Environment

- Python 3.10+
- Jupyter Notebook / JupyterLab
- GPU-enabled runtime recommended for finetuning and larger-model safety experiments

Install dependencies according to notebook imports used in your environment (for example: `transformers`, `datasets`, `peft`, `torch`, `pandas`, `numpy`, `evaluate`).

## Data and Output Management Notes

- Keep large checkpoints and heavy intermediate artifacts out of Git when possible.
- Store long-run experiment outputs in dedicated result folders for reproducibility.
- If sharing safety-related generations, use private channels/repositories when needed.

## Reproducibility Tips

- Keep notebook parameters (seed, model id, dataset split) explicit in each run.
- Save run metadata with outputs (timestamp, checkpoint id, eval setting).
- Use consistent naming for result folders so cross-run comparisons remain traceable.
- When comparing checkpoints, change one factor at a time (for example: only LoRA rank).
- Keep a single shared evaluation template so team members run the same protocol.
