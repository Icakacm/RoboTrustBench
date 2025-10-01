# RoboTrust: Evaluating the Interaction Trustworthiness of Multi-modal Large Language Models in Embodied Agents

## Project Overview

RoboTrust is an interactive benchmark designed to comprehensively evaluate the trustworthiness of multimodal large language models (MLLMs) in embodied agents. It systematically assesses the trust attributes of embodied agents when performing tasks in physical environments, providing researchers and developers with a standardized evaluation tool. Its core features include:



*   **5 Core Evaluation Dimensions**: Truthfulness, Safety, Fairness, Robustness, and Privacy

*   **12 Subtask Categories**: Covering diverse application scenarios and task types for granular, multi-dimensional assessment

*   **150 Interactive Evaluation Tasks**: Offering rich physics-based simulation scenarios to ensure comprehensive and practical evaluation

RoboTrust enables objective measurement of the trustworthiness of embodied agents through standardized evaluation workflows and quantitative metrics, facilitating the development of more reliable AI systems.

## Prerequisites

RoboTrust is built on EB-Habitat and relies on Habitat 2.0 for physics-based simulation. Before installing RoboTrust, ensure the following dependencies are properly installed:

### 1. Install Habitat-Sim (Core Physics Simulation Engine)

### 2. Install Habitat-Lab (Embodied Task Framework)

## Installation Requirements

After installing the prerequisites, you can deploy the RoboTrust evaluation environment in one step using the following commands:



1.  **Clone the RoboTrust Repository** 



```
cd Robotrustbench
```



1.  **Install Dependencies via requirements.yaml**



```
# 1. Create a conda environment from the configuration file (env name is defined in the yaml; modify if needed)

conda env create -f requirements.yaml

# 2. Activate the RoboTrust dedicated environment

conda activate robotrust  # If you modified the env name in the yaml, update this accordingly
```

## Evaluation Instructions

### 1. Supported Models

RoboTrust supports evaluation for **19 state-of-the-art open-source/closed-source multimodal LLMs**, including (but not limited to) GPT series, Claude series, LLaVA series, and other mainstream embodied AI models.

### 2. API Key Setup (Required for Closed-Source Models)

For closed-source models that require API calls, configure environment variables before evaluation (example for OpenAI models):



```
# For Linux/Mac systems

export OPENAI_API_KEY="your-actual-api-key-here"

# For Windows (Command Prompt)

set OPENAI_API_KEY=your-actual-api-key-here

# For Windows (PowerShell)

$env:OPENAI_API_KEY="your-actual-api-key-here"
```

For other closed-source models (e.g., Claude), refer to their official documentation to configure API key environment variables.

## Evaluation Entries (By Dimension)

RoboTrust provides separate entry scripts for each of the 5 evaluation dimensions. Below are complete example commands for each dimension:

### 1. Truthfulness Evaluation (Entry: main_truthfulness.py)



```
python -m main_truthfulness \\
 env=eb-hab \\
 model_name="$model_name"   # Replace with the model to evaluate (e.g., "gpt-4o") \\
 exp_name='Trust_truthfulness3'   # Experiment name (used for result storage naming)
 eval_sets='["truthfulness"]'   # Dataset for truthfulness evaluation
  n_shots=0   # Number of few-shot examples (0 = zero-shot evaluation)
  dataset_name=dataset.yaml   # Dataset configuration file
action_step=single   # Action step mode (single = single-step action, multi = multi-step action)
  trust=true   # Enable trustworthiness evaluation logic
max_episode_steps=20  # Maximum steps allowed per evaluation task
```

### 2. Safety Evaluation (Entry: main_safety.py)



```
python -m main_safety \\
env=eb-hab \\
model_name="$model_name" \\
exp_name='Trust_safety' \\
eval_sets='["safety_test"]'   # Dataset for safety evaluation
n_shots=0 \\
dataset_name=dataset.yaml \\
action_step=single \\
trust=true \\
max_episode_steps=20
```

### 3. Fairness Evaluation (Entry: main_fairness.py)



```
python -m main_fairness \\
env=eb-hab \\
model_name="$model_name" \\
exp_name='Trust_fairness' \\
eval_sets='["fairness_test"]' \  # Dataset for fairness evaluation
n_shots=0 \\
dataset_name=dataset.yaml \\
action_step=single \\
trust=true \\
max_episode_steps=20
```

### 4. Robustness Evaluation (Entry: main_robust.py)



```
python -m main_robust \\
env=eb-hab \\
model_name="$model_name" \\
exp_name='Trust_robustness' \\
eval_sets='["robust_test"]' \  # Dataset for robustness evaluation
n_shots=0 \\
dataset_name=dataset.yaml \\
action_step=single \\
trust=true \\
max_episode_steps=20
```

### 5. Privacy Evaluation (Entry: main_privacy.py)



```
python -m main_privacy \\
env=eb-hab \\
model_name="$model_name" \\
exp_name='Trust_privacy' \\
eval_sets='["privacy_test"]' \  # Dataset for privacy evaluation
n_shots=0 \\
dataset_name=dataset.yaml \\
action_step=single \\
trust=true \\
max_episode_steps=20
```

## Configuration Parameter Explanation



| Parameter           | Description                                                                                                    |
| ------------------- | -------------------------------------------------------------------------------------------------------------- |
| `model_name`        | Name of the model to evaluate (must match the supported model list, e.g., "gpt-4o", "llava-v1.6")              |
| `exp_name`          | Experiment name (used to distinguish different evaluation tasks; results are named after this)                 |
| `eval_sets`         | List of evaluation datasets (must match dataset names defined in `dataset.yaml`)                               |
| `n_shots`           | Number of few-shot examples (0 = zero-shot, 1/5 = one-shot/five-shot; requires dataset support)                |
| `dataset_name`      | Path to the dataset configuration file (default: `dataset.yaml`; update if the path is modified)               |
| `action_step`       | Action step mode (`single` = single-step action evaluation, `multi` = multi-step continuous action evaluation) |
| `trust`             | Whether to enable trustworthiness evaluation logic (`true` = enable, `false` = basic task evaluation only)     |
| `max_episode_steps` | Maximum number of steps allowed per evaluation task (task terminates if exceeded; default: 20 steps)           |

## Evaluation Result Storage

After evaluation, all results (logs, metric reports, task records) are automatically saved to the following directory:



```
running/rb_habitat/\<exp_name>/
```

where `<exp_name>` is the experiment name set in the evaluation command. The result files include:



*   `metrics_summary.json`: Summary of evaluation metrics (precision, recall, F1 score, and other trustworthiness metrics)

*   `task_logs/`: Detailed execution logs for each task (action records, model outputs, environment feedback)

*   `config.yaml`: Backup of the full configuration for the current evaluation task (for reproducibility)