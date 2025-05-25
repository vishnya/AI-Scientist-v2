
# Meta-Learning for Data-Efficient Molecular Modeling

## Keywords

* **In-silico Small Molecule Modeling**
* **Few-Shot Learning**
* **Meta-Learning**
* **Surrogate Modeling**
* **Molecular Representations**

## TL;DR

Apply meta-learning to small-molecule property prediction to enable accurate “surrogate” models with minimal data. By leveraging prior tasks and self-supervised pretraining, the approach aims to improve low-resource molecular predictions without any wet-lab experiments.

## Abstract

Machine learning models for small-molecule properties often struggle when only limited training data are available. This project proposes a **meta-learning framework for data-efficient molecular modeling**, enabling accurate in-silico surrogate models even in low-data regimes. The key idea is to train a graph-based model across many property prediction tasks so that it can rapidly adapt to new tasks with only a handful of molecules (few-shot learning). Recent work has shown that meta-learning with graph neural networks (GNNs) can significantly improve prediction accuracy in such scenarios. Building on public multi-task chemistry datasets (e.g. MoleculeNet benchmarks or the Therapeutic Data Commons), we will combine meta-learning with self-supervised pretraining on large unlabeled molecular libraries to initialize robust representations. During meta-training, the model will learn to **quickly generalize** to new property prediction tasks by simulating many train/test splits of known benchmarks. We hypothesize that this approach will outperform conventional single-task training and even rival large-scale pretraining methods with far fewer resources. Prior studies (e.g. Meta-MGNN) have already demonstrated improved few-shot performance on molecular tasks, suggesting the viability of our approach. The fully automated Sakana pipeline can carry out this research end-to-end: from hypothesis generation, to model development (using open-source GNN libraries), to experimentation on benchmark datasets, all in silico. Expected outcomes include a validated strategy for **low-resource molecular property prediction**, with results showing that a meta-learned surrogate model achieves strong accuracy on novel tasks using only a small fraction of the data required by conventional models. Such findings would contribute a novel direction for AI-driven drug discovery and materials science, where labeled data are often scarce but rapid in-silico evaluation is invaluable. All experiments will use public data (e.g. toxicity and solubility datasets) and open-source tools, ensuring reproducibility and compatibility with the AI Scientist v2 (Sakana) automated research pipeline.

**References:** MoleculeNet benchmark highlights the challenge of data scarcity in molecular ML. Guo *et al.* (2021) introduced a meta-learning GNN (Meta-MGNN) that outperforms prior methods on few-shot molecular property prediction. These inspire our low-resource approach, aligned with Sakana’s strengths in surrogate modeling and representation learning.

