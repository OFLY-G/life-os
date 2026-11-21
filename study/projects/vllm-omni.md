# vLLM-Omni 贡献（主线）

> 状态：❄️ 期末冻结到 12/2，**12/3 恢复**
> 目标：3 个有实质内容、我自己讲得清的合入 PR。一次只推进一个。合不合入由维护者决定，不承诺日期。
> 仓库：https://github.com/vllm-project/vllm-omni

## 现状核实（2026-11-21，main @ dbd2566）
- **Qwen3-Omni 已经被完整支持**：仓库里约 159 个相关文件，包括离线/在线示例、recipe、e2e 和精度测试、性能设计文档、async chunk、CUDA Graph、TP、w4a16 / NVFP4 量化测试、全双工、多副本、ROCm / Ascend。
- 所以「从零适配 Qwen3-Omni」这个题目已经不存在。技术报告没白读：它正是理解 Thinker → Talker → Code2Wav 多阶段流水的背景，做性能 PR 用得上。
- **调整后的方向**：在 Qwen3-Omni 这条已有链路上做有测量证据的改进（正确性、性能、测试缺口），也就是大会上大家说的「有深度的 PR」。

### Qwen3-Omni 在 vLLM-Omni 里的结构
来源：仓库内 `.claude/skills/vllm-omni/references/models/qwen3-omni.md`

| 阶段 | 类型 | 代码 | 输出 |
|---|---|---|---|
| 0 thinker | LLM_AR | `qwen3_omni_moe_thinker.py` | 文本 + hidden states |
| 1 talker | LLM_AR | `qwen3_omni_moe_talker.py`（+ code predictor MTP） | RVQ codec codes |
| 2 code2wav | LLM_GENERATION | `qwen3_omni_code2wav.py` | 波形 |

代码目录：`vllm_omni/model_executor/models/qwen3_omni/`；阶段间交接：`stage_input_processors/qwen3_omni.py`。

### 硬件现实
- 默认部署是 **2×H100**（thinker 一张，talker + code2wav 一张）或 **1×H200**。3060 6GB 跑不了完整链路。
- 所以 PR1 / PR2 优先选能在 CPU 或小 GPU 上验证的题；PR3（性能）等实验问题明确后再租卡。

## 三个 PR 的递进
| PR | 范围 | 要交出的证据 |
|---|---|---|
| PR1 | 一个明确的小问题：指标、参数传递、边界错误、测试缺口 | 修复前失败的用例 + 修复 + 回归测试 |
| PR2 | 在同一模块往深做：完整数据路径、异常处理、兼容性 | 正常和异常路径都验证 |
| PR3 | 一个测出来的性能问题（Qwen3-Omni 链路） | 正确性 + 前后对照 + 适用范围 + 真实调用场景 |

## 已做过的调查（Codex，main @ 9954edf，2026-11 中旬）
- 结论：当时**合格候选 0 个**，没有进入实现。
- 已排除：
  - Diffusion benchmark 请求超时可配置（#5548）：open PR #9646 已在做
  - TeaCache 系数估计测试缺口（#5145）：#9715 已改并补了测试
  - 错误 `ref_audio` 类型导致 HTTP 500：被证伪，vLLM v0.33.0 已经把 TypeError 映射成 400
  - 无音频请求污染 RTF：#10011 已合入；Realtime 格式测试：#10012
- 当时建议继续看：TTS benchmark 的「结果生成 → JSON → 报告」路径（`bench_tts.py`、`plot_results.py` 及对应测试）
- 学习材料（已合入，不能当任务）：#7624（结果字段遗漏导致误判失败）、#10137（参数没传到执行逻辑）、#10147（未初始化数据导致 NaN）、#9099
- 仓库规则：DCO sign-off；`precheck-pr` 要求本人回答 4 个问题；披露 AI 辅助；如实报告验证结果
- 仓库自带 Claude Code skills（`.claude/skills/`：precheck-pr、review-pr、omni-perf-opt、omni-perf-ceiling、vllm-omni-test 等），用 Claude Code 打开仓库就能用

## 值得调查的线索（仓库文档里写的，**还没验证有没有人在做**）
- 「Duplex performance under MRv2 is not established」：MRv2 下全双工的性能还没有数据
- TTS 优化设计文档里的 eager 示例比 deploy YAML 旧（文档与配置不一致）
- 开启 async_chunk 时，部分 realtime 路由不可用（见 recipe）

## 12/3 恢复时的第一步（已经替你选好）
1. 读 `docs/design/qwen3_omni_tts_performance_optimization.md` 的 Summary 和「Stacked optimization summary」（前 120 行左右），弄清已经做了哪些优化：Pipeline Batching、CUDA Graph、Async Chunk + Streaming、Code Predictor re-prefill + torch.compile、Dynamic Initial Chunk
2. 读上面那份 79 行的 Qwen3-Omni skill reference（Known quirks、Tuning levers）
3. 写一页笔记：三阶段图 + 每个阶段已经做过的优化 + 你的疑问。完成标准：能对着图讲 3 分钟
4. 然后基于最新 main 重新做一轮候选调查，选出 PR1

## 停车场（方向锁期间不做）
- vllm-metal：见 INBOX
