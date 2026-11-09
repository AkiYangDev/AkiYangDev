<p align="center">
  <img src="assets/hello.svg" width="100%" alt="Hi, I'm Aki! LLM inference runtimes and Ascend NPU kernels." />
</p>

<p align="center"><b>English</b> · <a href="README.zh-CN.md">简体中文</a></p>

<p align="center">
  <a href="#upstream-work">🛠️ Upstream work</a> &nbsp; · &nbsp;
  <a href="https://github.com/AkiYangDev/mini-deepinfer">🐋 mini-DeepInfer</a> &nbsp; · &nbsp;
  <a href="https://akiyangdev.github.io/llm-inference-notes/">📚 Engineering notes</a>
</p>

<img align="right" src="assets/whale-girl.png" width="128" alt="Community fan art of DeepSeek's whale girl" />

## 👋 I'm Aki

**LLM inference systems & Ascend NPU operators**

I work on SGLang and sgl-kernel-npu, connecting serving-runtime behavior with the operators underneath it: MoE expert migration, speculative-decoding state, quantized attention, and numerical correctness.

My contributions span implementation, bug diagnosis, reproducible checks, and technical documentation for **DeepSeek-V4 / V4.1 on Ascend**.

`Python` `PyTorch / torch_npu` `CANNBot-DSL` `HCCL / DeepEP`

<br clear="both" />

<a id="upstream-work"></a>

## 🛠️ Upstream engineering

### DeepSeek-V4.1 · quantized Indexer & sparse MLA

**[sgl-kernel-npu #964](https://github.com/sgl-project/sgl-kernel-npu/pull/964)** &nbsp; ✅ Merged · 2026-11-09 · Co-developed with [ClownBin](https://github.com/ClownBin)

Integrated **seven CANNBot-DSL operator APIs**—four compute operators and three metadata builders—for MXFP4 K-cache updates, two-stage sparse indexing, and mixed FP8/FP4 MLA.

My integration work covers API and dispatch contracts, independent Torch references, tests, bilingual API docs, and a backport that pads K-projection backing storage for tail tiles. [Contribution commits](https://github.com/sgl-project/sgl-kernel-npu/pull/964/commits)

### Dynamic EPLB · expert migration & statistics ownership

**[SGLang #47574](https://github.com/sgl-project/sglang/pull/47574)** &nbsp; ✅ Merged · 2026-11-02

Implemented dynamic expert load balancing on Ascend, connecting valid-route statistics, expert-weight migration, and bounded temporary memory.

**Bug discovery → merged community fix:** I identified and reproduced empty draft records consuming the target model's EPLB history ([issue #48603](https://github.com/sgl-project/sglang/issues/48603)) and supplied the recorder-lifecycle fix in [#47922](https://github.com/sgl-project/sglang/pull/47922); the community integrated that core fix in [#48751](https://github.com/sgl-project/sglang/pull/48751), authored by ch-wan and merged on 2026-11-06.

<details>
<summary>🔬 What made this bug subtle?</summary>

Draft execution can use `TARGET_VERIFY`, so the guard must follow the runner's role. Disabling counting hooks inside the model still lets the outer recorder reset counters and append an empty observation.

The fix has two parts: disable recording **before** entering the forward context, and make an already-disabled recorder skip reset, collection, and append. The issue includes a current-main reproducer and published 16-rank service records.

</details>

**Open follow-up:** [#48835 · Preserve arch35 expert storage during migration](https://github.com/sgl-project/sglang/pull/48835).

## 🔧 Selected patches

| Area | Engineering change | PR |
| --- | --- | --- |
| PD state transfer | Peer-local C4 rows and empty state components across PP stages | [#47934](https://github.com/sgl-project/sglang/pull/47934) / [#47937](https://github.com/sgl-project/sglang/pull/47937) / [#47938](https://github.com/sgl-project/sglang/pull/47938) |
| Decode memory | Bound DeepSeek-V4 SWA storage by request slots | [#47740](https://github.com/sgl-project/sglang/pull/47740) |
| Numerical precision | Preserve Softplus accuracy in GDN gating and decode | [kernel #933](https://github.com/sgl-project/sgl-kernel-npu/pull/933) |
| Sparse attention | Exclude padded lanes from non-power-of-two TopK reduction | [kernel #935](https://github.com/sgl-project/sgl-kernel-npu/pull/935) |
| Quantization | Preserve AWQ fallback dtype and correct GPTQ zero-point offsets | [#47639](https://github.com/sgl-project/sglang/pull/47639) / [#47637](https://github.com/sgl-project/sglang/pull/47637) |
| Benchmark timing | Record PD decode handoff time for non-streaming requests | [#47772](https://github.com/sgl-project/sglang/pull/47772) |

<sub>Selected patches and the arch35 follow-up were Open when checked on 2026-11-09. Follow each PR for its latest status.</sub>

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

[More authored upstream PRs](https://github.com/pulls?q=is%3Apr+author%3AAkiYangDev+org%3Asgl-project+is%3Aopen)

## 🐋 Projects & writing

### [mini-DeepInfer](https://github.com/AkiYangDev/mini-deepinfer)

A runnable teaching inference engine built around DeepInfer's execution design: scheduling, batching, KV-slot reclamation, execution across iterations, and speculative decoding. Start on CPU, then generate text with real Qwen3.5 weights on Ascend 910C.

### [LLM Inference Notes](https://akiyangdev.github.io/llm-inference-notes/)

Chinese engineering articles connecting SGLang source code with DeepSeek Attention / MoE / DSpark, distributed state ownership, quantization, profiling, and measured tuning results.

<p align="center">
  <img src="assets/whale-smile.png" width="70" alt="A cheerful chibi DeepSeek whale girl" /><br />
  <b>Always happy to talk inference, kernels, and a well-reproduced bug 🐋</b><br />
  <sub>DeepSeek whale girl · Community fan art</sub>
</p>
