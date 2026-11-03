# Cross-Modal Safety-Component Steering for Audio-Language Model Jailbreak Defense

Supplementary material for the paper by **Ping Xie and Xiaosong Liu**.

**[Read the supplementary appendix (PDF)](appendix.pdf)**

This repository distributes the supplementary appendix, including experimental protocols, component-level analyses, robustness evaluations, and resource measurements. The PDF contains a table of contents for navigation and references for the methods discussed.

## Method overview

Audio-language models process acoustic information that a transcript alone may not preserve, including speaker overlap, replay, masking, and room acoustics. Cross-Modal Safety-Component Steering is a white-box inference-time defense that uses access to internal activations to estimate safety directions for individual decoder heads and architecture-native audio-interface components. It selects a sparse set of intervention sites using intervention effects, audio evidence, benign sensitivity, and selection stability, then calibrates their deployment under safety and utility constraints.

The paper evaluates three policies:

- **Local steering:** applies calibrated updates at selected audio-conditioned states during prefill and the first eight generation steps.
- **Conditional steering:** activates decoder-component updates according to a calibrated risk gate.
- **Family-routed steering:** uses the risk gate and a router to select among six attack-family-specific directions for active decoder interventions.

The policies preserve base-model weights, prompts, and decoding settings. Evaluation covers Qwen2-Audio-7B-Instruct and MOSS-Audio-8B-Instruct, six non-adaptive attack families, and three benign listening tasks. Qwen2-Audio is also evaluated under held-out acoustic shifts and adaptive waveform attacks.

## Main results

The paper reports the following local-steering results on the primary non-adaptive evaluation:

| Model | ASR without defense | ASR with local steering | BUP with local steering |
|---|---:|---:|---:|
| Qwen2-Audio-7B-Instruct | 25.4% | 10.4% | 96.1% |
| MOSS-Audio-8B-Instruct | 23.8% | 12.1% | 95.3% |

**ASR** is attack success rate; lower is better. **BUP** is average benign task-score retention relative to the undefended model; higher is better. BUP is distinct from the benign false-refusal rate.

On Qwen2-Audio, conditional steering reaches 97.5% BUP. Local steering adds 1.6 ms median wrapper overhead under the primary timing protocol: one A100 80 GB GPU, BF16, and batch size one. This overhead excludes the base-model forward pass. The appendix reports separate measurements for other workloads and timing boundaries.

## Guide to the appendix

The appendix expands the Qwen2-Audio analyses. Section numbers below follow the PDF's table of contents.

| Sections | Contents |
|---|---|
| 1–2 | Definitions, direction estimation, ranking weights, and the short intervention window |
| 3 | Related methods, calibrated internal controls, and the paper-based SARSteer adaptation |
| 4 and 11 | Adaptive waveform attacks, expanded optimization budgets, and candidate retention |
| 5 and 12 | Online latency, memory, offline discovery and calibration, and input/output-length scaling |
| 6–8 | Teacher-forced versus autoregressive diagnostics, window length, long-form tasks, gating, and position masks |
| 9–10 | Component inventories, ranking sensitivity, and attack-family and benign-task breakdowns |

## Interpreting the evaluations

The appendix specifies the data partitions, fitting and calibration procedures, attack budgets, and profiling workloads associated with each analysis. Adaptive evaluations retain substantial residual risk and distinguish the initial evaluation from the expanded-budget stress test. Comparator results should be read with their stated fitting-data and search budgets; small differences in aggregate counts do not establish statistical superiority.

## Contact

For questions about the supplementary material, contact the corresponding author, [Xiaosong Liu](mailto:liuxiaosong@chinatelecom.cn).
