# Nova Research

### Metastable failures in prefix-cached LLM agent serving

**Akshat Valse**, Iowa State University. Honors undergraduate pursuing concurrent degrees in Statistics, Mathematics, Data Science and Computer Science; expected May 2028.
Research with **Dr. Ali Jannesari**, SwaPP Lab (systems, parallel computing, ML efficiency).

*This is a public overview. The code and data repository is private while the paper is in preparation. Contact me for access or details.*

---

## The question
AI agents work in loops: they call an LLM, run a tool, then call the LLM again, re-sending the whole growing conversation each time. Serving engines such as SGLang and vLLM keep that shared context in a GPU **KV cache**, which makes follow-up turns far cheaper than the first.

GPU memory is limited, though. When too many agent sessions compete for it, a session's cached context can be evicted while the agent is waiting on a tool, so its next turn has to recompute everything. That extra work keeps the cache full, and more sessions lose their context in turn.

**This project studies whether a short burst of traffic can push an otherwise healthy serving system into a self-sustaining slow state, a *metastable failure*, and which scheduling and admission mechanisms prevent it.**

## What the project involves
- **Simulation.** A discrete-event simulator of a continuous-batching LLM serving engine with prefix caching. It includes scheduling policies, cache-eviction rules, memory-allocation modes, and re-implementations of recently proposed mechanisms.
- **Theory.** A queueing-theoretic "thrashing curve" model, connecting classical results on working sets and metastable failures to agent workloads. It predicts when caching helps, when it collapses, and when admission control restores stability.
- **Real-system experiments.** A load-testing harness that drives a production serving engine (SGLang, a 7B-parameter model, NVIDIA A100 GPUs) on Iowa State's **Nova HPC cluster** through SLURM. Experiments use simulated multi-turn agent sessions with controlled bursts and paired control conditions.
- **Early finding.** On the real engine, cache reuse collapses sharply once concurrent agent sessions exceed the cache's working set, and throughput drops severalfold. This qualitatively matches the simulator. Experiments testing whether the collapse persists after the traffic burst ends are in progress.

## How the research is run
- **Pre-registered predictions.** Experiment outcomes are predicted and committed before the runs, then checked against the registered criteria.
- **Independent verification.** Every result is recomputed from raw data with separate code, and simulation runs are reproduced bit-for-bit from recorded commits.
- **Full provenance.** Every reported number traces to a results directory, a code commit and a cluster job ID.

## Skills and tools
Python (NumPy, SciPy, pandas, asyncio), discrete-event simulation, queueing theory and stochastic processes, statistical experiment design, HPC with SLURM, GPU/CUDA software environments, LLM serving systems (SGLang), Prometheus metrics, and pytest.

## Status
Active (Fall 2026). Target: arXiv preprint and a systems/ML workshop submission.
