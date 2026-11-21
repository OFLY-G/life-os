# 个人档案
> 最后更新：2026-11-21。来源：ChatGPT「建立个人AI主档」、Codex「推进 vLLM-Omni 首个 PR」「整理算子优化项目建议」三段对话，以及本次会话。

## 基本情况
- 学校：NTU（新加坡）。课程代码 EE64xx（学位、专业待确认）
- 本学期 5 门课：6102、6427、6406、6402（讲过加法器）、6407（已考完）；课名待补
- 设备
  - Windows 笔记本：RTX 3060 Laptop 6GB，WSL Ubuntu，VS Code，CUDA Toolkit 13.1，g++ 13.3
  - MacBook Air：Apple M5，16GB，macOS 26.5.2；目前只有 Command Line Tools，没有完整 Xcode
- 时间：学期中每周能给项目 12 小时以上；必要时可以租 GPU

## 目标
- 11/21–12/2：四门期末正常发挥（11/26 6102、11/27 6427、11/28 6406、12/2 6402）
- 接下来几个月：vLLM-Omni 3 个合入 PR（一次一个）；把 vllm_studies 整理成第一版投递材料；投 AI Infra / 推理优化实习（什么时候开始投：待补）
- 更远：AI Infra / 推理优化方向的工作（例如 NVIDIA）；秋招

## 技能现状（2026-11）
- 编程：C++ 熟于 Python；Python、pytest、异步编程边用边学
- 推理框架：nano-vLLM 类项目 V2 主线读完，包括生成、Paged KV、连续批处理、Chunked Prefill
- CUDA：Add（正确性 + 可靠计时）；读完 Mark Harris 的 Reduction；懂 warp/block/SM、合并访存、bank conflict、有效带宽
- 模型：读过 Qwen3-Omni 技术报告
- 并行：PP / TP / SP 的基本概念（vLLM Meetup 时学的）

## 我希望 AI 怎么教我
- 先一句结论，再用小数组、小例子或图解释，最后对应代码
- 明确区分容易混的概念（例如线程编号、数组下标、元素值）
- 一次只讲当前一步；我要能自己讲清楚改动，不要直接给看不懂的大 diff
- 不要假设我看过旧聊天，以本仓库的文件为准

## 资料在哪（原件不搬，只建索引）
| 资料 | 位置 |
|---|---|
| 推理框架项目 | GitHub：vllm_studies |
| 算子练习工程 | GitHub：operator-learning；WSL：`/home/ofly/operator-learning` |
| 算子学习总计划 | Notion：https://app.notion.com/p/4104c555681481a0a4d4f2a415c603a6 |
| 旧版个人主档 | ChatGPT 生成的《个人AI主档 v0.1.md》（有的话放进 `life/`，Claude 会合并进来） |
| vLLM-Omni 调查笔记 | Codex 云端：`contribution-notes/vllm-omni/`（摘要已整理进 `study/projects/vllm-omni.md`） |
| 技术博客 | https://dlog.com.cn/（推理框架和「手撕算子」都在这里学） |
| 日历 | Google Calendar（主日历）。Claude 建的期末事件，描述里都带「life-os 期末计划」 |
