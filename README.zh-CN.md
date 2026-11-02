<p align="center">
  <img src="assets/hello.svg" width="100%" alt="Hi, I'm Aki! 欢迎来到我的推理小实验室。" />
</p>

<p align="center"><a href="README.md">English</a> · <b>简体中文</b></p>

<p align="center">
  <a href="https://akiyangdev.github.io/llm-inference-notes/">📚 技术笔记</a> &nbsp; · &nbsp;
  <a href="https://github.com/AkiYangDev/mini-deepinfer">🐋 mini-DeepInfer</a> &nbsp; · &nbsp;
  <a href="https://github.com/pulls?q=is%3Apr+author%3AAkiYangDev+is%3Amerged">🌟 已合入的 PR</a>
</p>

<img align="right" src="assets/whale-girl.png" width="155" alt="DeepSeek 鲸鱼娘社区同人立绘" />

## 👋 这里是 Aki

写推理代码，查精度问题，盯性能时间线，也把踩过的坑整理成笔记。

最近主要围着 **SGLang、Ascend 910C 和 DeepSeek** 打转：

- 🧩 读懂调度、KV Cache、MoE 和投机解码。
- 🔬 排查算子精度、缓存与计算图问题。
- 🚀 做专家负载均衡、显存优化和性能分析。
- ✍️ 写能顺着读、动手跑的文章与小项目。

**LLM inference engineer · SGLang & Ascend NPU contributor**

<br clear="both" />

## 🌟 已合入的贡献

**[SGLang #47574 · Ascend 动态 EPLB](https://github.com/sgl-project/sglang/pull/47574)** &nbsp; 🎉 Merged · 2026-11-02

为 SGLang 接入 Ascend 动态专家负载均衡，打通有效路由统计、专家权重迁移与临时显存控制。

## 🔧 精选 PR

| 方向 | 改动 | PR |
| --- | --- | --- |
| 专家负载均衡 | 隔离 draft 统计，复用兼容的 TP 通信组 | [SGLang #47595](https://github.com/sgl-project/sglang/pull/47595) |
| DeepSeek-V4 显存 | 按请求槽位约束 decode SWA 存储 | [SGLang #47740](https://github.com/sgl-project/sglang/pull/47740) |
| 算子数值精度 | 修复 GDN gating / decode 的 Softplus 精度 | [sgl-kernel-npu #933](https://github.com/sgl-project/sgl-kernel-npu/pull/933) |
| 稀疏注意力 | 修复非 2 的幂 TopK 下的 padding 归约问题 | [sgl-kernel-npu #935](https://github.com/sgl-project/sgl-kernel-npu/pull/935) |
| 量化正确性 | 保留 AWQ fallback 权重类型；修正 GPTQ 零点偏移 | [#47639](https://github.com/sgl-project/sglang/pull/47639) / [#47637](https://github.com/sgl-project/sglang/pull/47637) |
| 压测计量 | 记录非流式请求的 PD decode 接管时间 | [SGLang #47772](https://github.com/sgl-project/sglang/pull/47772) |

<sub>上表 PR 在 2026-11-02 核对时均为 Open，最新进度见各 PR。</sub>


截至 **2026-11-02**，共提交 **30 个上游 PR：1 个已合入、28 个 Open、1 个关闭未合入**，包含下方列出的早期 EPLB 提交记录。

<details>
<summary>📦 更多 sglang 贡献 (12 PRs)</summary>

- [#47721 · 按服务上下文限制 DeepSeek-V4 RoPE 表大小](https://github.com/sgl-project/sglang/pull/47721) · Open
- [#47720 · 约束 DeepSeek-V4 prefill query 中间张量](https://github.com/sgl-project/sglang/pull/47720) · Open
- [#47718 · 释放 DeepSeek-V4 临时 RoPE 表](https://github.com/sgl-project/sglang/pull/47718) · Open
- [#47713 · 复用 DeepSeek-V4 verify 序列长度](https://github.com/sgl-project/sglang/pull/47713) · Open
- [#47665 · 用带步长的 arange 生成压缩位置](https://github.com/sgl-project/sglang/pull/47665) · Open
- [#47663 · 在 prefill metadata 中复用 CPU query 长度](https://github.com/sgl-project/sglang/pull/47663) · Open
- [#47659 · 在 CPU 回收时保留 MLA NZ 缓存数据](https://github.com/sgl-project/sglang/pull/47659) · Open
- [#47656 · 为 BF16 与 INT8 专家正确应用 SwiGLU clamp](https://github.com/sgl-project/sglang/pull/47656) · Open
- [#47654 · 在物理 host 槽位中保留 sparse-KV 共享前缀](https://github.com/sgl-project/sglang/pull/47654) · Open
- [#47647 · 移除未使用的 packed indexer cache 分配](https://github.com/sgl-project/sglang/pull/47647) · Open
- [#47634 · 在 TP all-gather 后正确切片 MLA latent K](https://github.com/sgl-project/sglang/pull/47634) · Open
- [#47629 · 遵循 simple TopK 的打分方式与路由缩放](https://github.com/sgl-project/sglang/pull/47629) · Open

</details>

<details>
<summary>📦 更多 sgl-kernel-npu 贡献 (8 PRs)</summary>

- [#936 · 为旧版 add RMSNorm 缓存键加入 dtype](https://github.com/sgl-project/sgl-kernel-npu/pull/936) · Open
- [#934 · 以 FP32 计算无权重 RMSNorm](https://github.com/sgl-project/sgl-kernel-npu/pull/934) · Open
- [#932 · 规范化非连续的 GDN gating 输入](https://github.com/sgl-project/sgl-kernel-npu/pull/932) · Open
- [#930 · 保留跨计算图重放的 matmul tiling 配置](https://github.com/sgl-project/sgl-kernel-npu/pull/930) · Open
- [#926 · 为 indexer tiling 缓存键补齐稀疏属性](https://github.com/sgl-project/sgl-kernel-npu/pull/926) · Open
- [#925 · 处理大于 16 token 的 cache-location 窗口](https://github.com/sgl-project/sgl-kernel-npu/pull/925) · Open
- [#924 · 处理 argmax probability 中前置 tile 全部被屏蔽的情况](https://github.com/sgl-project/sgl-kernel-npu/pull/924) · Open
- [#922 · 保持 arch22 compressor 在不同 batch 下的累加顺序](https://github.com/sgl-project/sgl-kernel-npu/pull/922) · Open

</details>

<details>
<summary>🗂️ 早期 EPLB 提交记录（2 个 PR）</summary>

- [SGLang #47266](https://github.com/sgl-project/sglang/pull/47266) · Open
- [SGLang #46233](https://github.com/sgl-project/sglang/pull/46233) · 已关闭，未合入

最终合入的 Ascend 动态 EPLB 实现在上方的 [#47574](https://github.com/sgl-project/sglang/pull/47574)。

</details>

[全部上游 PR](https://github.com/pulls?q=is%3Apr+author%3AAkiYangDev+org%3Asgl-project) · [已合入的上游 PR](https://github.com/pulls?q=is%3Apr+author%3AAkiYangDev+org%3Asgl-project+is%3Amerged)

## 🌱 自己种的小项目

### 🐋 [mini-DeepInfer](https://github.com/AkiYangDev/mini-deepinfer)

**读懂 DeepInfer，从一条请求开始。**

一个精简、可运行的教学推理引擎：先在 CPU 看调度、组批和 KV 槽位回收，再用真实 Qwen3.5 权重在 Ascend 910C 上生成文本，逐步理解跨轮执行与投机解码。

### 📚 [LLM Inference Notes](https://akiyangdev.github.io/llm-inference-notes/)

**把“能跑起来”继续追到“为什么这样跑”。**

记录 SGLang 源码、DeepSeek-V4 Attention / MoE / DSpark、Ascend 部署、Profiling 与调优复盘，把代码、实际结果和判断依据放在一起。

<p align="center">
  <img src="assets/whale-smile.png" width="78" alt="笑得很开心的 Q 版 DeepSeek 鲸鱼娘" /><br />
  <b>欢迎来聊推理、源码，或者一起捉一只 bug 🐋</b><br />
  <sub>DeepSeek 鲸鱼娘 · 社区同人</sub>
</p>
