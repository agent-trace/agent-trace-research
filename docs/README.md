# Agent Trace Research 文档索引

正式文档按主题分为项目背景、JSON 存储、记忆生成与 ARC-AGI-3 四组。实验代码和复现入口位于仓库的 `experiments/`（语料画像脚本位于 `experiments/payload-profile/`），运行数据与内部证据保存在 gitignored 的 `docs/temp/`。

## 项目背景

| 文档 | 内容 |
|---|---|
| [OTel、OTLP、Collector、GenAI 与 Langfuse 学习指南](project-background/otel-langfuse-study-guide.md) | Trace 数据模型、采集链路和相关项目概念 |
| [标准 openGauss 行存复现指南](project-background/trace-ingestion-demo-blue-zone-guide.md) | 历史固定版本的 openGauss 三表摄入 Demo |
| [GV xstore 复现指南](project-background/trace-ingestion-demo-yellow-zone-guide.md) | GV 环境、定制 Collector 构建和 dstore 验收 |

## JSON 存储

| 类型 | 文档 | 内容 |
|---|---|---|
| 设计 | [JSON 存储设计决策](json-storage/design-decision.md) | openGauss、XStore 与 ClickHouse 的逻辑模型、参数取值与判据、真实语料画像和证据缺口 |
| 原理 | [openGauss JSONB 与 ClickHouse Native JSON](json-storage/stage2-representation-principles.md) · [阶段三原理与设计](json-storage/stage3-payload-principles.md) | 四种 JSON 存储结构的写入、物理存储、后台维护、查询和恢复流程；长载荷四种布局在行存与列存中的物理组织、外置对象组件与场景设计 |
| 调研 | [JSON 存储设计调研](json-storage/design-survey.md) | 代表性系统、论文、当前项目状态和设计方向 |
| 实验设计 | [阶段一](json-storage/stage1-multifield-design.md) · [阶段二](json-storage/stage2-representation-design.md) · [阶段二调优矩阵](json-storage/stage2-tuning-design.md) · [阶段三](json-storage/stage3-payload-design.md) | 多字段 JSON、跨引擎表示比较、定向调优和长 payload 布局 |
| 实验报告 | [阶段一](json-storage/stage1-multifield-report.md) · [阶段二](json-storage/stage2-representation-report.md) · [阶段三（x86）](json-storage/stage3-payload-x86-report.md) · [阶段三（XStore，ARM 主机 A）](json-storage/stage3-payload-xstore-report.md) | 已完成实验的证据、数据结果、分析和适用范围 |
| 汇报材料 | [阶段二组内汇报辅助材料](json-storage/stage2-representation-briefing.md) · [阶段三组内汇报](json-storage/stage3-payload-briefing.md) | 核心流程与场景化比较摘要；阶段三三台主机的原理、环境、逐场景结果与跨主机对照 |

JSON 存储流程图位于 `json-storage/assets/`，由原理文档、报告和汇报材料共同使用。

## 记忆生成

| 类型 | 文档 | 内容 |
|---|---|---|
| 调研 | [基于 Agent Trace 生成记忆：调研索引](memory-generation/research/README.md) | 研究问题与范围、三份调研报告的分工与跨报告的主要发现 |
| 调研 | [生成方法调研（研究、实现与产品）](memory-generation/research/memory-generation-methods-2026-10-09.md) | 从轨迹生成经验记忆的研究、开源组件与实现、产品中的记忆机制与相邻可迁移方向，含完整术语与记忆分类 |
| 调研 | [信号与选样调研（trace 挖掘、归因评估、用户偏好、data agent）](memory-generation/research/memory-generation-signals-2026-10-09.md) | trace 库挖掘与近似 trace 归类、失败归因与评估、用户偏好与纠正、data agent 的经验记忆 |
| 调研 | [输入、存储与评测调研](memory-generation/research/memory-generation-input-storage-evaluation-2026-10-09.md) | trace 预处理、trace 标识与内容采集、生成侧模型接口、存储溯源与生命周期、评测与公开数据集分析 |

## ARC-AGI-3 与 VISTA

| 类型 | 文档 | 内容 |
|---|---|---|
| 说明 | [VISTA 运行时数据流与逻辑流](arcagi/vista-runtime-data-flow.md) | ARC-AGI-3 与 VISTA 背景，模型可见的输入与约束，单步时序，Agent 数据（会话上下文、截图、事件表、笔记、恢复内容）的写入时点与读取方式，会话交接，与 trace 数据模型的对应，以及实现组件 |
| 设计 | [VISTA 运行数据的 OpenTelemetry 存储设计](https://github.com/agent-trace/VISTA/blob/trace-v5.4/docs/trace-storage-design.md)（位于 VISTA trace 版仓库，tag `trace-v5.4`；部署与使用见同目录 `arc3-trace-usage.md`） | `--store pg` 下即时数据与审计数据经 OTel 写入 `events` 宽表：两条导出通道与 Collector pipeline、表与 span 设计、读屏障、原文归档与还原、运行结束时的入库核对、基于 trace 的断点续跑、验证结果与运行方式 |
| 报告 | [VISTA trace 版在 ARC-AGI-3 上的实验报告](https://github.com/agent-trace/VISTA/blob/arc3-trace/docs/arc3-experiments/arc3-trace-experiment-report.md)（位于 VISTA trace 版仓库 `docs/arc3-experiments/`） | GPT-6 Luna 在 25 个公开游戏上两阶段运行的结果与官方基线对照，DeepSeek-V4.1-Flash 补充对比，存储等价性、开销与归档验证，执行异常、限制与后续测试 |
| 汇报 | [VISTA trace 版实验：实现与结果](https://github.com/agent-trace/VISTA/blob/arc3-trace/docs/arc3-experiments/arc3-trace-briefing.md) | 面向已了解 VISTA 的读者：trace 版数据流与读写逻辑、续跑原理、实验结果与边界 |
| 诊断 | [GPT-6 Luna 推理与动作效率诊断](https://github.com/agent-trace/VISTA/blob/arc3-trace/docs/arc3-experiments/luna-reasoning-diagnostics-20261016.md) | 25 个游戏的可见输出、笔记版本与规则误判，9 个案例的本地验证 |

实验报告的证据快照（manifest 指定字段、官方基线、只读查询与采集脚本）位于同一目录的 `evidence/`。VISTA 示意图位于 `arcagi/assets/`。

## 临时资料

- `temp/` 保存本地运行清单、原始结果和内部核对材料；该目录不作为正式交付入口。
