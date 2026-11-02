<p align="center">
  <img src="assets/hello.svg" width="100%" alt="Hi, I'm Aki! Welcome to my little inference lab." />
</p>

<p align="center"><b>English</b> · <a href="README.zh-CN.md">简体中文</a></p>

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

Follow-up: [SGLang #47595](https://github.com/sgl-project/sglang/pull/47595) · Open

## 🔧 Selected pull requests

| Area | Change | PR |
| --- | --- | --- |
| DeepSeek-V4 memory | Bound decode SWA storage by request slots | [SGLang #47740](https://github.com/sgl-project/sglang/pull/47740) |
| Numerical precision | Fix Softplus accuracy in GDN gating and decode | [sgl-kernel-npu #933](https://github.com/sgl-project/sgl-kernel-npu/pull/933) |
| Sparse attention | Exclude padded lanes from non-power-of-two TopK reduction | [sgl-kernel-npu #935](https://github.com/sgl-project/sgl-kernel-npu/pull/935) |
| Quantization | Preserve AWQ fallback weight dtype; correct GPTQ zero-point offset | [#47639](https://github.com/sgl-project/sglang/pull/47639) / [#47637](https://github.com/sgl-project/sglang/pull/47637) |
| Benchmark metrics | Record PD decode handoff time for non-streaming requests | [SGLang #47772](https://github.com/sgl-project/sglang/pull/47772) |

<sub>These PRs were Open when checked on 2026-11-02. Follow each PR for its latest status.</sub>


<details>
<summary>📦 More sglang contributions (12 PRs)</summary>

- [#47721 · Bound DeepSeek-V4 RoPE tables by serving context](https://github.com/sgl-project/sglang/pull/47721) · Open
- [#47720 · Bound DeepSeek-V4 prefill query intermediates](https://github.com/sgl-project/sglang/pull/47720) · Open
- [#47718 · Release temporary DeepSeek-V4 RoPE tables](https://github.com/sgl-project/sglang/pull/47718) · Open
- [#47713 · dsv4: Reuse verify sequence lengths](https://github.com/sgl-project/sglang/pull/47713) · Open
- [#47665 · dsv4: Generate compression positions with stepped arange](https://github.com/sgl-project/sglang/pull/47665) · Open
- [#47663 · dsv4: Reuse CPU query lengths for prefill metadata](https://github.com/sgl-project/sglang/pull/47663) · Open
- [#47659 · MLA: preserve NZ cache data during CPU retraction](https://github.com/sgl-project/sglang/pull/47659) · Open
- [#47656 · moe: Honor SwiGLU clamp for BF16 and INT8 experts](https://github.com/sgl-project/sglang/pull/47656) · Open
- [#47654 · sparse-kv: Preserve shared prefixes in physical host slots](https://github.com/sgl-project/sglang/pull/47654) · Open
- [#47647 · dsv4: Avoid unused packed indexer cache allocation](https://github.com/sgl-project/sglang/pull/47647) · Open
- [#47634 · attention: Slice MLA latent K after TP all-gather](https://github.com/sgl-project/sglang/pull/47634) · Open
- [#47629 · moe: honor simple TopK scoring and routing scale](https://github.com/sgl-project/sglang/pull/47629) · Open

</details>

<details>
<summary>📦 More sgl-kernel-npu contributions (8 PRs)</summary>

- [#936 · include dtypes in legacy add RMSNorm cache key](https://github.com/sgl-project/sgl-kernel-npu/pull/936) · Open
- [#934 · compute weightless RMSNorm in float32](https://github.com/sgl-project/sgl-kernel-npu/pull/934) · Open
- [#932 · normalize strided gating inputs](https://github.com/sgl-project/sgl-kernel-npu/pull/932) · Open
- [#930 · preserve tiling configurations across graph replays](https://github.com/sgl-project/sgl-kernel-npu/pull/930) · Open
- [#926 · include sparse attributes in the tiling cache key](https://github.com/sgl-project/sgl-kernel-npu/pull/926) · Open
- [#925 · handle cache-location windows larger than 16 tokens](https://github.com/sgl-project/sgl-kernel-npu/pull/925) · Open
- [#924 · handle fully masked leading tiles in argmax probability](https://github.com/sgl-project/sgl-kernel-npu/pull/924) · Open
- [#922 · preserve arch22 accumulation order across batch sizes](https://github.com/sgl-project/sgl-kernel-npu/pull/922) · Open

</details>

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
