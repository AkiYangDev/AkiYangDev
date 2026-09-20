# Hi, I'm Aki Yang

**Backend engineering → LLM inference infrastructure**

I'm a backend engineer with a background in Java and cloud services, now exploring how LLM inference systems work—from request scheduling and KV cache management to execution on Ascend NPUs.

我是一名有 Java 云服务开发背景的工程师，目前正在深入学习大模型推理基础设施。最近主要围绕 **SGLang、Ascend NPU 和投机推理**，结合部署实践阅读源码，把请求如何调度、数据如何流动、模型如何执行串起来。

## Current focus / 当前关注

- **SGLang internals** — request lifecycle, scheduling, and the path from a request to model execution. 关注请求生命周期、调度与模型执行链路。
- **Ascend NPU** — model deployment, environment setup, and troubleshooting. 从实际部署出发，理解环境依赖与运行问题。
- **Speculative decoding** — draft and verification, token acceptance, and KV cache updates. 学习草稿生成、验证和缓存状态如何衔接。
- **Distributed inference** — tensor/data parallelism, data ownership, and communication correctness. 理解 TP/DP 下的数据归属与通信关系。

## How I learn / 学习方式

Start with a working example, trace the source, and check each explanation against code or an experiment.

从跑通一个例子开始，沿着真实调用链读代码，再用小实验验证理解。我喜欢用清晰的文字、调用链图和具体的 tensor shape 解释问题，也在练习把排障过程整理成可复现的记录。

## Next up / 接下来

- 整理 SGLang + Ascend 部署笔记与请求执行链路分析。
- 用小实验补齐 Attention、KV Cache 与并行推理的基础。
- 从能够复现和解释清楚的问题开始，逐步参与开源贡献。

---

Learning in public, one working example at a time.
