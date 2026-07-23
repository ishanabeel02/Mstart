# M-START: Crime Forecasting Architecture

Co-authored IEEE-format research (Butt et al., 2025) proposing **M-START**, a spatiotemporal crime forecasting architecture that replaces START's self-attention mechanism with **Mamba State Space Model (SSM) blocks**.

## Motivation

Self-attention scales at **O(n²)** with sequence length, which becomes a bottleneck for spatiotemporal forecasting over long time horizons and dense spatial grids. M-START swaps this for Mamba SSM blocks, which scale at **O(n)** — aiming to preserve forecasting quality while cutting the computational cost as sequence length grows.

## What was done

- Replaced START's self-attention layers with Mamba SSM blocks
- Ran controlled experiments on real **NYPD crime data**
- Empirically validated the architecture against baseline performance
- Co-authored and formatted the paper in IEEE conference style (LaTeX)
- Produced benchmark figures and conducted reference audits
- Prepared and delivered a presentation on the architecture and findings

## Tech / Approach

| Component | Details |
|---|---|
| Base architecture | START (spatiotemporal crime forecasting) |
| Modification | Self-attention → Mamba SSM blocks |
| Complexity | O(n²) → O(n) |
| Dataset | Real NYPD crime records |
| Output | IEEE-format paper, benchmark figures |

## Results

Best validation MAE: **2.4857** (model checkpoint: `start_nyc.pt`, device: CUDA)

Runtime and memory scale as sequence length (`n`) grows — comparing self-attention against Mamba SSM blocks:

| n | Attn ms/iter | Attn mem (MB) | Mamba ms/iter | Mamba mem (MB) | Speedup | Mem reduction |
|---|---|---|---|---|---|---|
| 48 | 1.55 | 33.0 | 3.72 | 41.8 | 0.42x | 0.8x |
| 96 | 3.90 | 49.4 | 3.18 | 62.6 | 1.22x | 0.8x |
| 192 | 4.75 | 73.4 | 4.66 | 102.0 | 1.02x | 0.7x |
| 336 | 12.59 | 116.3 | 8.92 | 164.0 | 1.41x | 0.7x |
| 720 | 44.97 | 229.0 | 20.27 | 328.3 | 2.22x | 0.7x |
| 1440 | 161.53 | 425.8 | 50.94 | 635.3 | 3.17x | 0.7x |
| 2880 | 609.82 | 834.5 | 108.65 | 1243.6 | 5.61x | 0.7x |

At small sequence lengths, self-attention is actually faster (Mamba has more overhead at n=48). But the crossover happens quickly — by n=96 Mamba is already ahead, and the gap widens sharply as n grows: **5.61x faster at n=2880**, while using noticeably more memory in absolute terms but a smaller relative footprint at scale (~0.7x). This is the O(n) vs O(n²) tradeoff in practice — Mamba's advantage compounds with longer sequences, which is exactly the regime spatiotemporal crime forecasting needs.

## Status

**Work in progress.** Experiments run and validated on real data; paper co-authored and formatted in IEEE style.

## Citation

M-START builds on the base START architecture:

```
U. M. Butt, S. Letchmunan, M. Ali and H. H. R. Sherazi, "START: A Spatiotemporal
Autoregressive Transformer for Enhancing Crime Prediction Accuracy," in IEEE
Transactions on Computational Social Systems, vol. 12, no. 6, pp. 4650-4664,
Dec. 2025, doi: 10.1109/TCSS.2025.3550196.

keywords: {Transformers; Spatiotemporal phenomena; Reactive power; Predictive
models; Market research; Time series analysis; Computational modeling; Data
models; Accuracy; Deep learning; Crime prediction; safe society; spatiotemporal;
time series analysis; transformers}
```
