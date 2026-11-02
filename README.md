<p align="center">
  <img src="assets/hello.svg" width="100%" alt="Hi, I'm Aki! 欢迎来到我的推理小实验室。" />
</p>

<p align="center"><b>简体中文</b> · <a href="README.en.md">English</a></p>

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

## 🔧 最近提交的 PR

| 方向 | 改动 | PR |
| --- | --- | --- |
| 专家负载均衡 | 隔离 draft 统计，复用兼容的 TP 通信组 | [SGLang #47595](https://github.com/sgl-project/sglang/pull/47595) |
| DeepSeek-V4 显存 | 按请求槽位约束 decode SWA 存储 | [SGLang #47740](https://github.com/sgl-project/sglang/pull/47740) |
| 算子数值精度 | 修复 GDN gating / decode 的 Softplus 精度 | [sgl-kernel-npu #933](https://github.com/sgl-project/sgl-kernel-npu/pull/933) |
| 稀疏注意力 | 修复非 2 的幂 TopK 下的 padding 归约问题 | [sgl-kernel-npu #935](https://github.com/sgl-project/sgl-kernel-npu/pull/935) |
| 量化正确性 | 保留 AWQ fallback 权重类型；修正 GPTQ 零点偏移 | [#47639](https://github.com/sgl-project/sglang/pull/47639) / [#47637](https://github.com/sgl-project/sglang/pull/47637) |
| 压测计量 | 记录非流式请求的 PD decode 接管时间 | [SGLang #47772](https://github.com/sgl-project/sglang/pull/47772) |

<sub>上表 PR 在 2026-11-02 核对时均为 Open，最新进度见各 PR。</sub>

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
