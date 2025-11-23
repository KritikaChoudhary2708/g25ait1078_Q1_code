# NAS-GA (Assignment A2)
Comprehensive README: what I changed, how to run, environment, and the results produced in this folder.

## Overview
This repository runs a miniature example of Neural Architecture Search (NAS) on evolving simple CNN architectures on CIFAR-like data using a Genetic Algorithm (GA). The GA deals with the tasks of initializing, fitness testing (train and validation), selection, crossover, mutation and elitism.

Intention of the changes provided (Q1A): Substitute default tournament selection with Roulette-Wheel (fitness-proportional) selection so that people are sampled proportionate to their (shifted) fitness.

## What changed
- `model_ga.py`:
	- Substituted previous tournament selection by Roulette-Wheel selection implementation in `selection()`.
	- The new selection:
		- non-negative fitness (negative fitness capped at 0.0)
		- positive fitness values (negative fitness capped at 0.0)
		- reverts to uniform choice when all the fitnesses are equal to or less than 0 (prevent divide-by-zero)
		- samples with replacement by using random.choices(..., weights=...) and produces deep copies of selected architectures.
	- Updating of progress log to show selection of roulette wheel on run.
		

Nor was there another algorithmic behaviour altered. The Fitness computation, crossover and mutation are as in the original implementation.

## How to run (recommended — WSL2 Ubuntu)

1. Prerequisites
   -   Python 3.x
   -   PyTorch
   -   CUDA (optional, for GPU acceleration)

2. Installation
   -   Clone or download the repository.
   -   Install the required dependencies:
    ```bash
    pip install -r requirements.txt
    ```

3. Execution

   To initiate the Neural Architecture Search, run the following command:

   ```bash
   python3 nas_run.py
   ```



## Results produced (this run)
The run completed under this folder and wrote outputs to:

```
AI/Assignment2/nas-ga-basic-main/outputs/run_1/
```

Key results (copied from `outputs/run_1/nas_run.log`):

- Device used: cpu
- Generations: 5, Population size: 10

- Final best architecture (genes):

```
{'num_conv': 4,
 'conv_configs': [
		{'filters': 128, 'kernel_size': 5},
		{'filters': 128, 'kernel_size': 3},
		{'filters': 128, 'kernel_size': 7},
		{'filters': 16,  'kernel_size': 5}
 ],
 'pool_type': 'max',
 'activation': 'relu',
 'fc_units': 64}
```

- Final metrics (from run):
	- Accuracy: 0.6970
	- Fitness: 0.6862
	- Total parameters: 1,078,522

Files created in the run (example):

- `outputs/run_1/nas_run.log` — full textual log of the run
- `outputs/run_1/generation_0.jsonl` — population snapshot after generation 0
- `outputs/run_1/generation_1.jsonl` — population snapshot after generation 1
- `outputs/run_1/...` — subsequent generation files
- `outputs/run_1/best_arch.pkl` — pickled best Architecture object

You can load the `best_arch.pkl` in Python to reconstruct and inspect the final `Architecture` object.

