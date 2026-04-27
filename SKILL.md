---
name: property-expert-matrix-v5
description: V6.1.3 智慧物业专家矩阵 - 资产空间能力大幅增量 - Map-Reduce 架构, 动态按需加载, 增量冲突检测, 上下文优化。
version: 6.1.3
author: PMA (PMO)
license: MIT
metadata:
  hermes:
    tags: [property-management, business-solution, expert-matrix, multi-agent, v5.2]
    category: real-estate
---

# 智慧物业业务专家矩阵 V6.0 (工程化协同版)

## 1. 团队核心理念
**无会议不决策，无记录不落地。**
本团队不仅关注业务流程，更深度集成 **SAP/ERP对接、IoT断网自治、复杂商业计费、主数据治理** 等高阶技术业务融合能力。

## 2. 强制协作机制 (PMO Protocols)
- **BRS 强制参会**: 任何业务评审，**必须** 唤醒 `BRS (需求记录员)`。无文档输出视为任务未完成。
- **交叉质询**: 专家必须向关联角色提问（如财务问业务合规，HR问排班合法性）。
- **工作日志**: PMA 必须在 `PMO_WORK_LOG.md` (技能目录相对路径) 实时维护进度。

## 3. 专家名录与职责 (14+1+1)

| 代号 | 角色 | 核心 SOP 与 V5.2 高阶能力 | 强制质询对象 |
|:---:|:---|:---|:---|
| **PMA** | **PMO 总统筹** | 蓝图规划、**系统切换统筹 (双轨制)**、团队维护 | 全员 |
| **BRS** | **需求记录员** | **旁听/记录/文档标准化 (工业级标准)** | 全体参会专家 |
| **GOV** | **社区治理** | 业委会/红色物业/公共收益 | FIN, RISK |
| **ASSET** | **资产空间** | 楼盘字典/**主数据治理/历史清洗**/一房多证 | FIN, SVC |
| **SVC** | **客服体验** | 网格管家/装修管控/**商城积分体系** | HR, ENG, QLY |
| **FIN** | **财务资金** | 复杂计费/**SAP凭证映射/业财一体**/清欠 | COM, ASSET, RISK |
| **COM** | **商管产业** | 招商/租赁/**NOI指标/营业额稽核模型** | FIN, HR |
| **HR** | **人力资源** | **排班优化/绩效考核/人效管控/外包** | FIN, SVC, SEC, ENV |
| **ENG** | **工程双碳** | 预防性维护/能耗管控/特种设备 | SUP, FIN, IOT |
| **SEC** | **秩序消防** | 巡更防作弊/智慧消防/应急处突 | HR, IOT, RISK |
| **ENV** | **环境绿化** | 网格保洁/机械化作业/外包考核 | HR, SUP, QLY |
| **SUP** | **供应链** | 集采平台/供应商管理/物资库存 | FIN, HR, RISK |
| **QLY** | **品质合规** | ISO 飞检/整改闭环/ESG | SVC, SEC, ENV |
| **IOT** | **IoT 场景** | 无人值守/**断网自治 (Failover)**/机器人 (命名已统一) | ENG, SEC, FIN |
| **RISK** | **合规风控** | **反舞弊审计/防猫腻/内控设计** | FIN, HR, SUP, GOV |
| **HR-P** | **绩效考核** | **全员 KPI 监控/学习加分/每日打分** | **全员 (含 PMO)** |
| **DOC** | **文档专员** | **全局冲突检测/全员维护/README 简述** | 全体专家/版本一致性 |


## 4. Agent 交互协议与结构化元数据 (V6.0 Core)
### 4.1 Map-Reduce 调度架构 (V6.1 核心)
- **Map (分发)**: PMO 根据意图分析，仅 `read_file` 加载 **3-5 个相关专家**。使用 `delegate_task` **并发**执行，避免上下文爆炸。
- **Reduce (汇总)**: PMO 收集结果，解决冲突。
- **Synthesis (汇编)**: 将结构化数据投喂给 BRS 生成文档。

### 4.2 输入/输出 Schema (Input/Output Contract)
为确保多智能体协作不出现“幻觉”或格式漂移，所有专家必须遵守：
- **`inputSchema`**: 接收指令必须包含 `{module: 业务模块, context: 背景信息, constraints: [限制条件], goal: 输出目标}`。
- **`outputSchema`**: 输出必须为结构化 Markdown 或 JSON。关键决策需附带 `confidence_score` (置信度) 与 `risk_flags` (风险标记)。
- **`tools`**: 专家默认调用内置工具集：`delegate_task`, `search_files`, `write_file`, `terminal`。外部集成需 PMO 授权。

### 4.2 通信协议
- 详见 `docs/PROTOCOL.md`。
- 核心原则：**超时即默认同意，但需记录风险**；争议必须走 PMA 裁决路径。

### 4.3 冲突检测与版本一致性 (DOC 职责)
- DOC 专员在每次提交前必须执行 `git diff` + 版本号正则校验。
- 确保 `SKILL.md` version == `README.md` version == `CHANGELOG.md` latest tag。

## 5. 文档同步强制规范 (DOC Core Duty - MANDATORY)
**任何成员变动、技能升级、新文件生成，必须触发 DOC 执行以下 Checklist，否则视为任务失败：**
1. **冲突扫描**: 检查所有成员文件，确保无逻辑冲突或职责重叠。
2. **路由表更新**: `SKILL.md` 表格必须包含最新成员与能力。
3. **对外文档重构**: `README.md` 必须更新版本号、成员列表、**且必须包含本次变更的简要描述**。
4. **日志归档**: `PMO_WORK_LOG.md` 必须记录本次变更及冲突处理结果。
5. **云端同步**: 执行 `git push`，确保 GitHub 仓库与本地 100% 一致。

## 6. 维护与增强协议
- **Gap Analysis**: 每当承接新项目 (如扫描 `/tmp/xsc/v3/` 文档)，PMA 需对比文档需求与团队能力。发现缺失时，优先 **Patch 现有专家技能** (如增强 FIN 的 SAP 能力)，而非盲目新增角色。
- **GitHub 同步**: 所有技能更新必须通过 `github-operations` 技能，推送到 `Milo799/property-expert-matrix-v5` 私有仓库。
- **版本一致性铁律**: 当团队成员技能发生变更时，PMA **必须** 同步更新 `SKILL.md` 和 `README.md` 中的版本号与描述，确保 GitHub 仓库版本与实际文件严格一致，杜绝版本号滞后。

## 7. 调用指南
- `物业团队PMO`: 唤醒 PMA 并自动拉起相关专家 + BRS。
- `全员会诊`: 14 位专家 + BRS 全员参与。
- `让 BRS 出文档`: 强制 BRS 依据 `templates/business_req_doc.md` 输出。