# Physics-Informed Trajectory Diffusion

**Conditional Denoising Diffusion Probabilistic Models (DDPMs) for multi-step 2D trajectory prediction**

## Overview

Predicting how a dynamical system evolves over time is challenging when the available physical model is approximate or incomplete. This project investigates whether learned models can correct a simplified physics-based forecast, with a particular focus on **conditional diffusion models**.

The task is to predict **10 future 2D positions from 4 initial observations** in a synthetic nonlinear dynamical system. Predictions use only information available at inference time: future ground-truth observations are not provided to the models.

This is a small-scale study of physics-informed trajectory generation and prediction—not an image-generation system, a complete robotic world model, or a robot controller.

## Research question

**Can a conditional DDPM learn trajectory corrections to an imperfect physical model, and how does it compare with deterministic residual learning and unconstrained diffusion?**

The study also examines performance under a **distribution shift** between training and evaluation conditions.

## Approaches compared

| Method | Description |
|---|---|
| **Simplified physics** | Predicts future positions using an intentionally incomplete dynamical model. |
| **Residual MLP** | Learns deterministic corrections to the physics-based trajectory. |
| **Standalone DDPM** | Learns future trajectories with a diffusion model without the simplified physics forecast. |
| **Physics-informed conditional DDPM** | Learns a distribution of trajectory residuals, conditioned on the simplified physical prediction. |

The conditional DDPM performs denoising in **2D trajectory space**, not in pixel or image space. “Physics-informed” here means conditioning on an approximate physical forecast; it does **not** imply that physical laws are strictly enforced.

## Experimental setup

- **Data:** synthetically generated nonlinear 2D trajectories.
- **Input:** 4 observed positions.
- **Prediction horizon:** 10 future positions.
- **Evaluation:** in-distribution (ID) and out-of-distribution (OOD) test sets.
- **Metric:** Average Displacement Error (ADE; lower is better).
- **Implementation:** Python and PyTorch; notebook-oriented workflow suitable for Google Colab.

## Results

| Method | ID ADE ↓ | OOD ADE ↓ |
|---|---:|---:|
| Simplified physics | 0.1178 | 0.1860 |
| **Residual MLP** | **0.0415** | **0.0886** |
| Standalone DDPM | 0.0824 | 0.1294 |
| Physics-informed conditional DDPM | 0.0635 | 0.1097 |

Relative to the **simplified-physics baseline**, the physics-informed DDPM reduced ADE by approximately:

- **46.1%** in-distribution.
- **41.0%** out-of-distribution.

In the reported run, the deterministic **residual MLP achieved the lowest ADE in both settings**. Thus, these results support the usefulness of learned corrections to incomplete physics, but **do not demonstrate that diffusion outperforms deterministic residual learning**.

A paired bootstrap of the test-set error difference between the physics-informed DDPM and the physics-only baseline yielded intervals of **[0.0507, 0.0579] (ID)** and **[0.0720, 0.0809] (OOD)**, in absolute ADE units. These intervals do not capture variability from training the models with different random seeds.

## Interpretation

The comparison suggests that incorporating an approximate physical forecast into trajectory diffusion can improve predictive accuracy over both the simplified-physics baseline and the standalone DDPM in this experimental setup. However, a simpler deterministic residual approach remains stronger on ADE. Evaluating the value of diffusion for **multimodal future predictions**, uncertainty calibration, and physical plausibility would require additional experiments.

## Limitations

- The environment and trajectories are **synthetic and two-dimensional**.
- The study does **not** evaluate visual object permanence, occlusion handling, persistent 3D state, or robotic control.
- Conditioning on physical predictions is not equivalent to hard physical constraints.
- Results reflect the reported experiment; multiple independent training runs would be needed to assess seed sensitivity.
- ADE measures predictive accuracy, not the full quality or diversity of a generative distribution.

## Running the experiment

Open the project notebook in **Google Colab** or a local Jupyter environment with Python and PyTorch installed. A **T4 GPU** is recommended for Colab experiments. Run the cells in order to generate the synthetic data, train the four approaches, and reproduce the evaluation and plots.


## Relevance to world models

Long-horizon prediction and consistency with physical dynamics are useful components of generative world modelling. This project focuses narrowly on **trajectory-level prediction under imperfect dynamics**. Extending it toward persistent world models would require visual or 3D state representations, object permanence across occlusions, and validation in robotic prediction and control.

## Author

**Binetou Cécile NIANG**
