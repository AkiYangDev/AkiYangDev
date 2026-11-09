<p align="center">
  <img src="assets/hello.svg" width="100%" alt="Hi, I'm Aki! 大模型推理运行时与 Ascend NPU 算子。" />
</p>

<p align="center"><a href="README.md">English</a> · <b>简体中文</b></p>

<p align="center">
  <a href="#user-content-upstream-work">🛠️ 上游贡献</a> &nbsp; · &nbsp;
  <a href="https://github.com/AkiYangDev/mini-deepinfer">🐋 mini-DeepInfer</a> &nbsp; · &nbsp;
  <a href="https://akiyangdev.github.io/llm-inference-notes/">📚 技术笔记</a>
</p>

<img align="right" src="assets/whale-girl.png" width="128" alt="DeepSeek 鲸鱼娘社区同人立绘" />

## 👋 这里是 Aki

**大模型推理系统与 Ascend NPU 算子开发**

主要参与 SGLang 和 sgl-kernel-npu，把服务运行时与底层算子连起来：MoE 专家迁移、投机解码状态、量化注意力，以及数值正确性。

围绕 **DeepSeek-V4 / V4.1 的 Ascend 实现**，做功能开发、bug 定位、可复现验证和技术文档。

`Python` `PyTorch / torch_npu` `CANNBot-DSL` `HCCL / DeepEP`

<br clear="both" />

<a id="upstream-work"></a>

## 🛠️ 上游工程贡献

### DeepSeek-V4.1 · 量化 Indexer 与稀疏 MLA

**[sgl-kernel-npu #964](https://github.com/sgl-project/sgl-kernel-npu/pull/964)** &nbsp; ✅ 已合入 · 2026-11-09 · 与 [ClownBin](https://github.com/ClownBin) 协作

将 **七个 CANNBot-DSL 算子 API** 接入公共框架，包括四个计算入口、三个 metadata 入口，覆盖 MXFP4 K-cache 更新、两级稀疏索引和 FP8/FP4 混合 MLA。

我的集成工作包括 API 与分发契约、独立 Torch 参考实现、测试、双语 API 文档，以及 K 投影尾块底层存储补齐的修复回移。[查看贡献提交](https://github.com/sgl-project/sgl-kernel-npu/pull/964/commits)

### 动态 EPLB · 专家迁移与统计归属

**[SGLang #47574](https://github.com/sgl-project/sglang/pull/47574)** &nbsp; ✅ 已合入 · 2026-11-02

为 Ascend 实现动态专家负载均衡，打通有效路由统计、专家权重迁移与临时显存控制。

**发现问题 → 社区合入修复：** 我发现并复现了 draft 空记录挤占 target EPLB 统计窗口的问题（[issue #48603](https://github.com/sgl-project/sglang/issues/48603)），并在 [#47922](https://github.com/sgl-project/sglang/pull/47922) 提供 recorder 生命周期修复；社区随后在 [#48751](https://github.com/sgl-project/sglang/pull/48751) 接入该核心修复，由 ch-wan 提交，于 2026-11-06 合入。

<details>
<summary>🔬 这个 bug 难在哪里？</summary>

Draft 执行也可能使用 `TARGET_VERIFY`，因此要按 runner 身份判断。只在模型内部关闭计数 hook，外层 recorder 仍会重置计数并追加一条空记录。

修复需要两处配合：在进入 forward 记录上下文之前关闭 draft 记录；已关闭的 recorder 跳过重置、收集和追加。Issue 中附有当前主干复现脚本与公开的 16-rank 服务采集记录。

</details>

**后续改进（Open）：** [#48835 · 在迁移时保留 arch35 专家的存储布局](https://github.com/sgl-project/sglang/pull/48835)。

## 🔧 精选修复与优化

| 方向 | 工程改动 | PR |
| --- | --- | --- |
| PD 状态传输 | 按对端局部行传输 C4，处理 PP 阶段的空状态分量 | [#47934](https://github.com/sgl-project/sglang/pull/47934) / [#47937](https://github.com/sgl-project/sglang/pull/47937) / [#47938](https://github.com/sgl-project/sglang/pull/47938) |
| Decode 显存 | 按请求槽位约束 DeepSeek-V4 SWA 存储 | [#47740](https://github.com/sgl-project/sglang/pull/47740) |
| 数值精度 | 保留 GDN gating / decode 的 Softplus 精度 | [kernel #933](https://github.com/sgl-project/sgl-kernel-npu/pull/933) |
| 稀疏注意力 | 排除非 2 的幂 TopK 中的 padding 归约干扰 | [kernel #935](https://github.com/sgl-project/sgl-kernel-npu/pull/935) |
| 量化正确性 | 保留 AWQ fallback 类型，修正 GPTQ 零点偏移 | [#47639](https://github.com/sgl-project/sglang/pull/47639) / [#47637](https://github.com/sgl-project/sglang/pull/47637) |
| 压测计时 | 记录非流式请求的 PD decode 接管时间 | [#47772](https://github.com/sgl-project/sglang/pull/47772) |

<sub>表中 PR 与 arch35 后续改进在 2026-11-09 核对时均为 Open，最新状态见各 PR。</sub>

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

[更多本人提交的上游 PR](https://github.com/pulls?q=is%3Apr+author%3AAkiYangDev+org%3Asgl-project+is%3Aopen)

## 🐋 项目与技术写作

### [mini-DeepInfer](https://github.com/AkiYangDev/mini-deepinfer)

围绕 DeepInfer 执行设计构建的可运行教学引擎，串起调度、组批、KV 槽位回收、跨轮执行和投机解码；先在 CPU 看清执行过程，再用真实 Qwen3.5 权重在 Ascend 910C 上生成文本。

### [LLM Inference Notes](https://akiyangdev.github.io/llm-inference-notes/)

把 SGLang 源码与 DeepSeek Attention / MoE / DSpark、分布式状态归属、量化、Profiling 和实测调优结果连起来的中文工程文章。

<p align="center">
  <img src="assets/whale-smile.png" width="70" alt="笑得很开心的 Q 版 DeepSeek 鲸鱼娘" /><br />
  <b>欢迎来聊推理、算子，或者一个能复现的 bug 🐋</b><br />
  <sub>DeepSeek 鲸鱼娘 · 社区同人</sub>
</p>
