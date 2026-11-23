# Agent Trace 摄入

本上下文描述 Agent Trace 从合成数据进入数据库并完成对账的项目语言。它用于统一跨仓库协作时对流程边界和验收对象的称呼。

## Language

**摄入链路**：
从 trace 数据进入 OTLP 发送端开始，到数据库确认写入结束的连续数据路径。
_Avoid_: 全链路、端到端系统

**摄入 Demo**：
使用固定输入、固定组件版本和确定性验收条件执行的一次摄入链路验证。
_Avoid_: benchmark、graph test

**数据生产端**：
生成 flat trace JSONL，并将其转换为 OTLP 后发送的 `trace-synthesis` 组件。
_Avoid_: exporter、Collector

**数据库适配边界**：
把 Collector 内的 trace 数据映射为三表 SQL 并写入数据库的 `exporter_demo` 组件。
_Avoid_: Collector、openGauss exporter 进程

**发送确认清单**：
记录已获得 OTLP HTTP 成功响应的 `(trace_id, span_id)` 集合，用于和数据库实际内容做差集对账。
_Avoid_: 数据库提交日志、benchmark report

**蓝区基线**：
使用标准 openGauss 6.0.0 和 `row` storage profile 建立的协议、DDL、写入及查询兼容性环境。
_Avoid_: xstore 等价环境

**黄区目标**：
使用 openGauss 兼容接口和 xstore dstore 存储形态的内网部署环境。
_Avoid_: 标准 openGauss

**三表契约**：
由 `traces`、`observations`、`scores` 构成的当前存储模型；摄入写入前两表并创建空的 `scores` 表。
_Avoid_: benchmark schema 版本、openGauss server 版本
