<p align="center">
  <img src="assets/hello.svg" width="100%" alt="Hi, I'm Aki! 大模型推理系统、Ascend 算子与实测性能。" />
</p>

<p align="center"><a href="README.md">English</a> · <b>简体中文</b></p>

<p align="center">
  <a href="#user-content-upstream-work">🌟 已合入成果</a> &nbsp; · &nbsp;
  <a href="https://github.com/AkiYangDev/mini-deepinfer">🐋 mini-DeepInfer</a> &nbsp; · &nbsp;
  <a href="https://akiyangdev.github.io/llm-inference-notes/">📚 技术笔记</a>
</p>

## 👋 这里是 Aki

**DeepSeek 推理系统与 Ascend NPU 工程**

`SGLang` `DeepSeek` `Ascend 910C`

<img align="right" src="assets/whale-girl.png" width="110" alt="DeepSeek 鲸鱼娘社区同人立绘" />

参与 **SGLang** 和 **sgl-kernel-npu**，在 Ascend 910C 上实现和排查 DeepSeek 推理链路。

上游工作涉及 **MoE / EPLB**、prefill/decode 状态传输、投机解码统计和 NPU 算子正确性，并用可复现测试与服务实测核验结果。

<br clear="both" />

<a id="upstream-work"></a>

## ✅ 已合入上游成果

### 🌟 Ascend EPLB 物理存储迁移

**[SGLang #49701](https://github.com/sgl-project/sglang/pull/49701)** &nbsp; ✅ 已合入 · 2026-11-13

以可写字节视图迁移完整专家物理存储，保留 NZ padding 和计算图引用的原分配；复用 TP/HCCL 通信，并隔离 draft 预热与捕获对 target 统计的影响。

**此前已合入的 EPLB 工作：** [#47574](https://github.com/sgl-project/sglang/pull/47574) 接入初始 Ascend 支持（11 月 2 日）；ch-wan 发起的 [#48751](https://github.com/sgl-project/sglang/pull/48751) 采用了我的核心 recorder 修复（11 月 6 日）。该问题由我在 [issue #48603](https://github.com/sgl-project/sglang/issues/48603) 发现并报告，原始修复见 [#47922](https://github.com/sgl-project/sglang/pull/47922)。

<picture>
  <source media="(max-width: 600px)" srcset="assets/eplb-storage-mobile.svg" />
  <img src="assets/eplb-storage.svg" width="100%" alt="把 NZ 与 packed 存储映射为可写专家字节行，交给迁移器处理，保留 padding 和模型原分配。" />
</picture>

**EPLB OFF → ON 实测：** 在公开的 DeepSeek-V4-Flash W8A8 PD 长文复制负载中，输出吞吐 **2,648.89 → 3,627.05 token/s（+36.93%）**，平均 E2E **50.49 → 36.39 s**；平均 TPOT 同时由 **8.85 升至 10.52 ms/token**。

<details>
<summary>🔬 实验配置、接受计数与延迟</summary>

成对试验使用 `08865d168`：Ascend 910C，P+D 共 16 张物理卡 / 32 个芯片，每端 TP16/EP16/attention-DP16，DSpark gamma 5 / verify 6，输入 16,384、输出 1,024 token，并发 128，关闭 prefix/index cache，两组实际 KV 容量对齐。

吞吐统计共同 **[60, 360) s** 窗口内实际收到的生成 token；延迟统计窗口内发起并成功完成的请求。

| 指标 | EPLB OFF | EPLB ON |
| --- | ---: | ---: |
| 输出吞吐，token/s | 2,648.89 | 3,627.05 |
| 平均 TTFT，s | 41.43 | 25.63 |
| 平均 TPOT，ms/token | 8.85 | 10.52 |
| 平均 E2E，s | 50.49 | 36.39 |
| 接受长度，token/verify | 5.988304 | 5.988304 |
| 实测严格 draft 接受率 | 100% | 100% |
| 正确 / 提议 draft token，延迟样本组 | 658,350 / 658,350 | 908,010 / 908,010 |
| 全程成功 / 失败请求 | 3,072 / 0 | 3,072 / 0 |

3,072 对生成文本逐一相同。ON 为 decode 输送了更大的实际 batch，吞吐提升伴随单请求 TPOT 的取舍。这里对比 EPLB 开关；旧 ND 路径与物理字节迁移的实现成本另见 PR。

</details>

### 更多已合入成果

- **A3 prefill/decode 状态 — [SGLang #47934](https://github.com/sgl-project/sglang/pull/47934)** · 11 月 12 日  
  以按行注册和对端局部寻址，修复 P/D 环大小不同时的 C4 状态交接。

- **算子 API 集成 — [kernel #1012](https://github.com/sgl-project/sgl-kernel-npu/pull/1012)** · 11 月 10 日  
  统一 wrapper、registry、测试与双语文档中的 `_dsl` 名称，保留实现逻辑。

- **DeepSeek-V4.1 算子 — [kernel #964](https://github.com/sgl-project/sgl-kernel-npu/pull/964)** · 11 月 9 日  
  与 ClownBin 协作集成七个 CANNBot-DSL 计算/metadata API，完善分发契约、Torch 参考、测试与 K 尾块存储修正。

## 🔧 进行中的修复与优化

<details>
<summary>精选待合入改动 · 状态传输、显存、精度与计时（9 PRs）</summary>

| 方向 | 工程改动 | PR |
| --- | --- | --- |
| PD 状态 | 保留 PP 阶段的空分量；在 AMD 上按请求局部行传输 C4 | [#47937](https://github.com/sgl-project/sglang/pull/47937) / [#47938](https://github.com/sgl-project/sglang/pull/47938) |
| Gateway 生命周期 | decode 提前失败时取消仍待处理的 prefill | [#49792](https://github.com/sgl-project/sglang/pull/49792) |
| Decode 显存 | 按请求槽位约束 DeepSeek-V4 SWA 存储 | [#47740](https://github.com/sgl-project/sglang/pull/47740) |
| 数值精度 | 保留 GDN gating / decode 的 Softplus 精度 | [kernel #933](https://github.com/sgl-project/sgl-kernel-npu/pull/933) |
| 稀疏注意力 | 排除非 2 的幂 TopK 中的 padding 归约干扰 | [kernel #935](https://github.com/sgl-project/sgl-kernel-npu/pull/935) |
| 量化正确性 | 保留 AWQ fallback 类型，修正 GPTQ 零点偏移 | [#47639](https://github.com/sgl-project/sglang/pull/47639) / [#47637](https://github.com/sgl-project/sglang/pull/47637) |
| 压测计时 | 记录非流式请求的 PD decode 接管时间 | [#47772](https://github.com/sgl-project/sglang/pull/47772) |

<sub>Open 状态核对于 2026-11-14，最新上游进度见各链接。</sub>

</details>

<details>
<summary>📦 更多待合入 SGLang 修复与优化（12 PRs）</summary>

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
<summary>📦 更多待合入 NPU 算子修复（8 PRs）</summary>

- [#936 · 为旧版 add RMSNorm 缓存键加入 dtype](https://github.com/sgl-project/sgl-kernel-npu/pull/936) · Open
- [#934 · 以 FP32 计算无权重 RMSNorm](https://github.com/sgl-project/sgl-kernel-npu/pull/934) · Open
- [#932 · 规范化非连续的 GDN gating 输入](https://github.com/sgl-project/sgl-kernel-npu/pull/932) · Open
- [#930 · 保留跨计算图重放的 matmul tiling 配置](https://github.com/sgl-project/sgl-kernel-npu/pull/930) · Open
- [#926 · 为 indexer tiling 缓存键补齐稀疏属性](https://github.com/sgl-project/sgl-kernel-npu/pull/926) · Open
- [#925 · 处理大于 16 token 的 cache-location 窗口](https://github.com/sgl-project/sgl-kernel-npu/pull/925) · Open
- [#924 · 处理 argmax probability 中前置 tile 全部被屏蔽的情况](https://github.com/sgl-project/sgl-kernel-npu/pull/924) · Open
- [#922 · 保持 arch22 compressor 在不同 batch 下的累加顺序](https://github.com/sgl-project/sgl-kernel-npu/pull/922) · Open

</details>

## 🐋 项目与工程笔记

### [mini-DeepInfer](https://github.com/AkiYangDev/mini-deepinfer)

围绕 DeepInfer 执行设计构建的可运行教学引擎，串起调度、组批、KV 槽位回收与投机解码，从 CPU 小例子走到 Ascend 910C 上的真实 Qwen3.5 文本生成。

### [LLM Inference Notes](https://akiyangdev.github.io/llm-inference-notes/)

把上游源码、可运行例子与实际测量连起来的中文文章。近期更新：

- [搬专家权重，还需要 NZ→ND 吗？](https://akiyangdev.github.io/llm-inference-notes/articles/performance/ascend-eplb-physical-storage-migration/)：padding、scratch、生命周期与更新成本。
- [图捕获以后，为什么还能改权重，却不能随便换张量？](https://akiyangdev.github.io/llm-inference-notes/articles/performance/graph-capture-tensor-storage/)：权重更新与捕获地址的关系。
- [权重搬完没报错，怎样证明模型还在算对的东西？](https://akiyangdev.github.io/llm-inference-notes/articles/performance/eplb-weight-migration-correctness/)：字节、量化 scale、映射与固定状态前向验证。

<p align="center">
  <img src="assets/whale-smile.png" width="70" alt="笑得很开心的 Q 版 DeepSeek 鲸鱼娘" /><br />
  <b>欢迎来聊推理、算子，或者一个能复现的 bug 🐋</b><br />
  <sub>DeepSeek 鲸鱼娘 · 社区同人</sub>
</p>

