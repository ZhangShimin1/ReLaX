# ReLaX: Reasoning with Latent Exploration for Large Reasoning Models

[Shimin Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang,+S), [Xianwei Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen,+X), [Yufan Shen](https://arxiv.org/search/cs?searchtype=author&query=Shen,+Y), [Ziyuan Ye](https://arxiv.org/search/cs?searchtype=author&query=Ye,+Z), [Jibin Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu,+J)

Affiliations: see the ReLaX paper for full institutional details.  
Corresponding: contact authors via the emails listed on the [arXiv page](https://arxiv.org/abs/2512.07558).

[![Code](https://img.shields.io/badge/Code-GitHub-black?logo=github)](.) 
[![Paper](https://img.shields.io/badge/arXiv-2512.07558-b31b1b.svg)](https://arxiv.org/abs/2512.07558)
[![Models & Data](https://img.shields.io/badge/🤗%20Models-ReLaX%20Collection-ffcc4d.svg)](https://huggingface.co/collections/SteveZ25/relax-reasoning-with-latent-exploration)

---

### TL;DR

**ReLaX is a training paradigm for Large Reasoning Models (LRMs) that regulates exploration directly in the latent space, using Koopman operator theory and a new Dynamic Spectral Dispersion (DSD) metric to mitigate entropy collapse in RL with verifiable rewards (RLVR). This leads to richer latent dynamics, more robust exploration, and stronger, more stable reasoning performance across text-only and multimodal benchmarks.**

---

### News

- **2025-12-08**: ReLaX paper released on arXiv `2512.07558`.
- **Code release**: We are preparing the training/evaluation code and configurations used in the paper. Stay tuned by watching this repository.

---

### Overview

Reinforcement Learning with Verifiable Rewards (RLVR) has recently emerged as a powerful way to improve the reasoning capabilities of LRMs. However, RLVR often suffers from **entropy collapse**: the policy prematurely converges to a narrow set of behaviors, limiting exploration and ultimately capping performance.

Most existing solutions operate at the **token level**, e.g., by manipulating token entropy. In contrast, **ReLaX** takes a **latent-dynamics-first** perspective:

- It models the hidden-state dynamics of LRMs using **Koopman operator theory** to obtain a linearized representation of the internal computation.
- It introduces **Dynamic Spectral Dispersion (DSD)**, a metric that quantifies the heterogeneity of latent dynamics and serves as a direct indicator of policy exploration.
- It uses DSD to explicitly regularize the exploration–exploitation tradeoff during RLVR policy optimization.

Across diverse reasoning benchmarks (both text-only and multimodal), ReLaX significantly alleviates entropy collapse and yields **state-of-the-art reasoning performance**, while maintaining training stability.

---

### Key ideas

- **Latent-space exploration signal**: Instead of only tracking token entropy, ReLaX monitors the diversity of hidden-state trajectories via Koopman-based spectral analysis.
- **Dynamic Spectral Dispersion (DSD)**: A principled metric that measures how heterogeneous the latent dynamics are, serving as a proxy for how broadly the policy is exploring.
- **Koopman linearization of hidden states**: ReLaX leverages Koopman operator theory to make the analysis and control of latent trajectories tractable.
- **Exploration–exploitation regulation in RLVR**: DSD is incorporated into the RLVR objective so that the model learns to explore sufficiently without degenerating into high-entropy noise.

---

### Method at a glance

At a high level, ReLaX works as follows:

1. **Base model**: Start from a pretrained Large Reasoning Model (e.g., an LLM or multimodal LRM) with strong base capabilities.
2. **RLVR setup**: Train the model with RLVR, using verifiable rewards (e.g., programmatic checkers, execution results, or external verifiers) for reasoning tasks.
3. **Koopman latent dynamics**:
   - Collect hidden-state trajectories during rollouts.
   - Fit a Koopman operator approximation to obtain a linearized latent dynamics model.
4. **Compute DSD**:
   - Use the Koopman spectrum to measure how diverse the latent dynamics are.
   - Higher DSD ⇒ richer exploration in the latent space.
5. **Training with ReLaX**:
   - Add a DSD-based term to the RL objective to encourage beneficial exploration.
   - Optimize the policy to jointly maximize verifiable rewards and maintain healthy latent exploration.

For full mathematical details and experimental setup, please refer to the paper:

- **ReLaX: Reasoning with Latent Exploration for Large Reasoning Models**  
  [`https://arxiv.org/abs/2512.07558`](https://arxiv.org/abs/2512.07558)

---

### Repository status

This repository is the **official project page** for ReLaX.

What you can find (or expect) here:

- **Paper**: Conceptual foundations, theory, and full experimental results ([arXiv:2512.07558](https://arxiv.org/abs/2512.07558)).
- **Checkpoints and datasets**: Curated via the Hugging Face collection  
  → [`ReLaX: Reasoning with Latent Exploration` on Hugging Face](https://huggingface.co/collections/SteveZ25/relax-reasoning-with-latent-exploration).
- **Code (coming soon)**:
  - Training recipes for RLVR with ReLaX.
  - Evaluation scripts and configs for the benchmarks used in the paper.

If you are interested in early access or collaboration, feel free to open an issue or reach out to the authors by email (see the arXiv page).

---

### How to use ReLaX (high-level guidance)

While the full code release is in progress, the ReLaX paradigm can be incorporated into an existing RLVR pipeline roughly as follows:

1. **Integrate latent logging**: Modify your RLVR training loop to log hidden states (or suitable projections) along each trajectory.
2. **Fit a Koopman operator**: Periodically fit a Koopman approximation to these trajectories (e.g., via least squares or spectral methods) to obtain a latent transition operator.
3. **Compute DSD**: From the Koopman spectrum, compute Dynamic Spectral Dispersion as a scalar measure of latent exploration.
4. **Augment the RL objective**: Add DSD (or a function of it) as an auxiliary term or constraint in your policy optimization step.
5. **Tune the tradeoff**: Adjust the strength of the DSD term to balance reward maximization and latent exploration.

The paper includes more formal definitions and experimental ablations that can guide a faithful implementation.

---

### Citation

If you find ReLaX or this repository useful in your research, please cite our paper:

```bibtex
@article{zhang2025relax,
  title   = {ReLaX: Reasoning with Latent Exploration for Large Reasoning Models},
  author  = {Zhang, Shimin and Chen, Xianwei and Shen, Yufan and Ye, Ziyuan and Wu, Jibin},
  journal = {arXiv preprint arXiv:2512.07558},
  year    = {2025}
}
```

