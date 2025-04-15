<div align="center">
  <a href="https://github.com/SakanaAI/AI-Scientist_v2/blob/main/docs/logo_v1.jpg">
    <img src="docs/logo_v1.png" width="215" alt="AI Scientist v2 Logo" />
  </a>
  <h1>
    <b>The AI Scientist-v2: Workshop-Level Automated</b><br>
    <b>Scientific Discovery via Agentic Tree Search</b>
  </h1>
</div>

<p align="center">
  📚 <a href="https://pub.sakana.ai/ai-scientist-v2/paper">[Paper]</a> |
  📝 <a href="https://sakana.ai/ai-scientist-first-publication/"> [Blog Post]</a> |
  📂 <a href="https://github.com/SakanaAI/AI-Scientist-ICLR2025-Workshop-Experiment"> [ICLR2025 Workshop Experiment]</a>
</p>

Fully autonomous scientific research systems are becoming increasingly capable, with AI playing a pivotal role in transforming how scientific discoveries are made.
We are excited to introduce The AI Scientist-v2, a generalized end-to-end agentic system that has generated the first workshop paper written entirely by AI and accepted through peer review.

This system autonomously generates hypotheses, runs experiments, analyzes data, and writes scientific manuscripts. Unlike [its predecessor (AI Scientist-v1)](https://github.com/SakanaAI/AI-Scientist), the AI Scientist-v2 removes reliance on human-authored templates, generalizes across Machine Learning (ML) domains, and employs a progressive agentic tree search, guided by an experiment manager agent.

> **Note:**
> The AI Scientist-v2 doesn’t necessarily produce better papers than v1, especially when a strong starting template is available. v1 follows well-defined templates, leading to high success rates, while v2 takes a broader, more exploratory approach with lower success rates. v1 works best for tasks with clear objectives and a solid foundation, whereas v2 is designed for open-ended scientific exploration.

> **Caution!**
> This codebase will execute Large Language Model (LLM)-written code. There are various risks and challenges associated with this autonomy, including the potential use of dangerous packages, uncontrolled web access, and the possibility of spawning unintended processes. Ensure that you run this within a controlled sandbox environment (e.g., a Docker container). Use at your own discretion.

## Table of Contents

1.  [Requirements](#requirements)
    *   [Installation](#installation)
    *   [Supported Models and API Keys](#supported-models-and-api-keys)
2.  [Run AI Scientist-v2 Paper Generation Experiments](#run-ai-scientist-v2-paper-generation-experiments)
3.  [Citing The AI Scientist-v2](#citing-the-ai-scientist-v2)
4.  [Frequently Asked Questions](#frequently-asked-questions)
5.  [Acknowledgement](#acknowledgement)

## Requirements

This code is designed to run on Linux with NVIDIA GPUs using CUDA and PyTorch.

### Installation

```bash
# Create a new conda environment
conda create -n ai_scientist python=3.11
conda activate ai_scientist

# Install PyTorch with CUDA support (adjust pytorch-cuda version for your setup)
conda install pytorch torchvision torchaudio pytorch-cuda=12.4 -c pytorch -c nvidia

# Install PDF and LaTeX tools
conda install anaconda::poppler
conda install conda-forge::chktex

# Install Python package requirements
pip install -r requirements.txt
```

### Supported Models and API Keys

#### OpenAI Models

By default, the system uses the `OPENAI_API_KEY` environment variable for OpenAI models.

#### Claude Models via AWS Bedrock

To use Claude models provided by Amazon Bedrock, install the necessary additional packages:
```bash
pip install anthropic[bedrock]
```
Next, configure valid [AWS Credentials](https://docs.aws.amazon.com/cli/v1/userguide/cli-configure-envvars.html) and the target [AWS Region](https://docs.aws.amazon.com/bedrock/latest/userguide/bedrock-regions.html) by setting the following environment variables: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_REGION_NAME`.

#### Semantic Scholar API (Literature Search)

Our code can optionally use a Semantic Scholar API Key (`S2_API_KEY`) for higher throughput during literature search [if you have one](https://www.semanticscholar.org/product/api). However, the system should work without it, though you might encounter rate limits. If you experience issues with Semantic Scholar, you can skip the literature search and citation phases during paper generation.

#### Setting API Keys

Ensure you provide the necessary API keys as environment variables for the models you intend to use. For example:
```bash
export OPENAI_API_KEY="YOUR_OPENAI_KEY_HERE"
export S2_API_KEY="YOUR_S2_KEY_HERE"
# Set AWS credentials if using Bedrock
# export AWS_ACCESS_KEY_ID="YOUR_AWS_ACCESS_KEY_ID"
# export AWS_SECRET_ACCESS_KEY="YOUR_AWS_SECRET_KEY"
# export AWS_REGION_NAME="your-aws-region"
```

## Run AI Scientist-v2 Paper Generation Experiments

You can specify the models used for the write-up and review phases via command-line arguments.
The configuration for the best-first tree search (BFTS) is located in `bfts_config.yaml`. Adjust parameters in this file as needed.

Key tree search configuration parameters in `bfts_config.yaml`:

-   `agent` config:
    -   Set `num_workers` (number of parallel exploration paths) and `steps` (maximum number of nodes to explore). For example, if `num_workers=3` and `steps=21`, the tree search will explore up to 21 nodes, expanding 3 nodes concurrently at each step.
    -   `num_seeds`: Should generally be the same as `num_workers` if `num_workers` is less than 3. Otherwise, set `num_seeds` to 3.
    -   Note: Other agent parameters like `k_fold_validation`, `expose_prediction`, and `data_preview` are not used in the current version.
-   `search` config:
    -   `max_debug_depth`: The maximum number of times the agent will attempt to debug a failing node before abandoning that search path.
    -   `debug_prob`: The probability of attempting to debug a failing node.
    -   `num_drafts`: The number of initial root nodes (i.e., the number of independent trees to grow) during Stage 1.

Example command to run AI-Scientist-v2 on one of the provided ideas (`i_cant_believe_its_not_better.json`). Please review `bfts_config.yaml` for detailed tree search parameters (the default config includes `claude-3-5-sonnet` for experiments).

```bash
python launch_scientist_bfts.py \
 --load_ideas "ai_scientist/ideas/i_cant_believe_its_not_better.json" \
 --load_code \
 --add_dataset_ref \
 --model_writeup o1-preview-2024-09-12 \
 --model_citation gpt-4o-2024-11-20 \
 --model_review gpt-4o-2024-11-20 \
 --model_agg_plots o3-mini-2025-01-31 \
 --num_cite_rounds 20
```

Once Stage 1 is complete, you will find a timestamped log folder inside the `experiments/` directory. Navigate to `experiments/"timestamp_ideaname"/logs/0-run/` within that folder to find the tree visualization file `unified_tree_viz.html`.

## Citing The AI Scientist-v2

If you use **The AI Scientist-v2** in your research, please cite our work as follows:

```bibtex
@misc{aiscientist_v2,
 title = {The {AI} Scientist-v2: Workshop-Level Automated Scientific Discovery via Agentic Tree Search},
 author = {Yamada, Yutaro and Lange, Robert Tjarko and Lu, Cong and Hu, Shengran and Foerster, Jakob and Clune, Jeff and Ha, David},
 year = {2025},
 month = {April},
 howpublished = {Sakana AI Blog Post. Available online: \url{https://pub.sakana.ai/ai-scientist-v2/paper}},
}
```

## Frequently Asked Questions

**Why wasn't a PDF or a review generated for my experiment?**

The AI Scientist-v2 completes experiments with a success rate that depends on the chosen foundation model, and the complexity of the idea. Higher success rates are generally observed when using powerful models like Claude 3.5 Sonnet for the experimentation phase.

**What is the estimated cost per experiment?**

Using Claude 3.5 Sonnet for the experimentation phase typically costs around $15–$20 per run. The subsequent writing phase adds approximately $5 when using the default models specified in the example command. Using GPT-4o for `model_citation` is recommended as it can help reduce writing costs.

**How do I run The AI Scientist-v2 for different subject fields?**

You can add a new JSON file to the `ai_scientist/ideas/` directory. For reference, take a look at the example: `ai_scientist/ideas/i_cant_believe_its_not_better.json`.

**What should I do if I have problems accessing the Semantic Scholar API?**

The Semantic Scholar API is used to assess the novelty of generated ideas and to gather citations during the paper write-up phase. If you don't have an API key, encounter rate limits, you may be able to skip these phases.

**I encountered a "CUDA Out of Memory" error. What can I do?**

This error typically occurs when the AI Scientist-v2 attempts to load or run a model that requires more GPU memory than available on your system. To resolve this, you can try updating your idea's JSON file (`ai_scientist/ideas/your_idea.json`) to suggest using a smaller model for the experiments or analysis.

## Acknowledgement

The tree search component implemented within the `ai_scientist` directory is built on top of the [AIDE](https://github.com/WecoAI/aideml) project. We thank the AIDE developers for their valuable contributions and for making their work publicly available.


## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=SakanaAI/AI-Scientist-v2&type=Date)](https://star-history.com/#SakanaAI/AI-Scientist-v2&Date)

