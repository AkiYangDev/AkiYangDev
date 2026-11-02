<p align="center">
  <img src="assets/hello.svg" width="100%" alt="Hi, I'm Aki! Welcome to my little inference lab." />
</p>

<p align="center"><a href="README.md">简体中文</a> · <b>English</b></p>

<p align="center">
  <a href="https://akiyangdev.github.io/llm-inference-notes/">📚 Engineering notes (Chinese)</a> &nbsp; · &nbsp;
  <a href="https://github.com/AkiYangDev/mini-deepinfer">🐋 mini-DeepInfer</a> &nbsp; · &nbsp;
  <a href="https://github.com/pulls?q=is%3Apr+author%3AAkiYangDev+is%3Amerged">🌟 Merged PRs</a>
</p>

<img align="right" src="assets/whale-girl.png" width="155" alt="Community fan art of DeepSeek's whale girl" />

## 👋 A little about me

I write inference code, debug numerical issues, inspect performance traces, and turn what I learn into engineering notes.

Lately, most of my work revolves around **SGLang, Ascend 910C, and DeepSeek**:

- 🧩 Understanding scheduling, KV caches, MoE, and speculative decoding.
- 🔬 Debugging operator precision, caches, and graph execution.
- 🚀 Working on expert load balancing, memory use, and inference performance.
- ✍️ Building small runnable projects and writing guides to go with them.

**LLM inference engineer · SGLang & Ascend NPU contributor**

<br clear="both" />

## 🌟 Merged contributions

**[SGLang #47574 · Dynamic EPLB on Ascend](https://github.com/sgl-project/sglang/pull/47574)** &nbsp; 🎉 Merged · 2026-11-02

Added dynamic expert load balancing on Ascend to SGLang, connecting valid-route statistics, expert-weight migration, and bounded temporary memory.

## 🔧 Recent pull requests

| Area | Change | PR |
| --- | --- | --- |
| Expert load balancing | Isolate draft statistics and reuse compatible TP groups | [SGLang #47595](https://github.com/sgl-project/sglang/pull/47595) |
| DeepSeek-V4 memory | Bound decode SWA storage by request slots | [SGLang #47740](https://github.com/sgl-project/sglang/pull/47740) |
| Numerical precision | Fix Softplus accuracy in GDN gating and decode | [sgl-kernel-npu #933](https://github.com/sgl-project/sgl-kernel-npu/pull/933) |
| Sparse attention | Exclude padded lanes from non-power-of-two TopK reduction | [sgl-kernel-npu #935](https://github.com/sgl-project/sgl-kernel-npu/pull/935) |
| Quantization | Preserve AWQ fallback weight dtype; correct GPTQ zero-point offset | [#47639](https://github.com/sgl-project/sglang/pull/47639) / [#47637](https://github.com/sgl-project/sglang/pull/47637) |
| Benchmark metrics | Record PD decode handoff time for non-streaming requests | [SGLang #47772](https://github.com/sgl-project/sglang/pull/47772) |

<sub>These PRs were Open when checked on 2026-11-02. Follow each PR for its latest status.</sub>

[All upstream PRs](https://github.com/pulls?q=is%3Apr+author%3AAkiYangDev+org%3Asgl-project) · [Merged upstream PRs](https://github.com/pulls?q=is%3Apr+author%3AAkiYangDev+org%3Asgl-project+is%3Amerged)

## 🌱 Things I'm growing

### 🐋 [mini-DeepInfer](https://github.com/AkiYangDev/mini-deepinfer)

**Learn DeepInfer, one request at a time.**

A small, runnable teaching inference engine: explore scheduling, batching, and KV slot reclamation on CPU, then generate text with real Qwen3.5 weights on Ascend 910C while learning execution across iterations and speculative decoding.

### 📚 [LLM Inference Notes](https://akiyangdev.github.io/llm-inference-notes/)

**From getting models to run to understanding how they run.**

Chinese engineering notes on SGLang internals, DeepSeek-V4 Attention / MoE / DSpark, Ascend deployment, profiling, and tuning, with code, measured results, and the reasoning behind each conclusion.

<p align="center">
  <img src="assets/whale-smile.png" width="78" alt="A cheerful chibi DeepSeek whale girl" /><br />
  <b>Let's talk inference, read some code, or catch a bug together 🐋</b><br />
  <sub>DeepSeek whale girl · Community fan art</sub>
</p>
