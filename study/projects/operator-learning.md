# CUDA 算子学习（辅线）

> 状态：❄️ 期末冻结。12/3 后每周 ≤3 小时，服务主线的性能 PR 和面试。
> 定位：不是第二条主线。目标是能看懂、测量、解释框架里的算子性能，而不是和 cuBLAS 比赛。

## 已完成
- CUDA Add：n = 31 / 32 / 33 / 1000 正确性 PASS；可靠计时（预热、重复、同步）〔已验证〕
  - 2026-11-07，3060 Laptop，float32，256 threads/block：
    - 1M 元素约 0.0445 ms，有效带宽约 283 GB/s
    - 16M 元素约 0.649 ms，有效带宽约 310 GB/s，大约是理论峰值 336 GB/s 的 92%（按 192-bit × 14 Gbps 估算，以显卡实际规格为准）
  - 结果目录：`/home/ofly/operator-learning/results/runs/20261107T075831Z.bgTtjv`
- 读完 Mark Harris 的 Reduction PDF（逐页讲解在 Notion）〔自述〕
- 搞懂的概念：thread / warp / block / SM 层次、grid-stride、合并访存、float4 向量化、bank conflict、有效带宽计算

## 进行中
- 博客 dlog.com.cn「手撕算子」：Element-wise ✅ → Reduce（https://dlog.com.cn/posts/cuda08/reduce01/ ，进行中）

## 下一步（解冻后）
1. Reduce 博客读完；自己实现共享内存版求和（验证 n = 1 / 31 / 32 / 33 / 1000）；两版计时对照
2. Softmax / RMSNorm（归约和逐元素的组合）
3. 之后看 PR3 需要什么，再决定往 GEMM 还是 Attention 走

## 资料与位置
- 本地工程：`/home/ofly/operator-learning`（WSL），里面有 `LEARNING_PLAN.md`、`notes/first_add_lab.md`
- 总计划（Notion）：https://app.notion.com/p/4104c555681481a0a4d4f2a415c603a6
- 10 月整理过三条网上建议（GEMM 做深 / 手搓 PTX-SASS / 按 bound 分析），结论：用「按 bound 分析」做方法论；SM100 / SM120 深挖放进停车场，等有明确的实验问题再租卡

## 简历上能写的（目前）
- 还不够独立成项目。先和 vllm_studies 合并成「推理框架 + 算子性能分析」的能力描述。
