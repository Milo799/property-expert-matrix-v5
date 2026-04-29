---
name: property-expert-matrix-v5
description: V7.0.0 智慧物业专家矩阵 - 深度进化/Token精简/知识卡片化/按需加载
version: 7.0.0
author: PMA (PMO)
license: MIT
metadata:
  hermes:
    tags: [property-management, business-solution, expert-matrix, multi-agent, v7]
    category: real-estate
---

# 智慧物业业务专家矩阵 V7.0.0 (深度进化/Token精简版)

## 1. 核心理念
**按需加载 · 深度优先 · Token极简**。每位专家浓缩为"知识卡片"：高密度结构化数据，零废话，只保留决策必需的框架/公式/规则。

## 2. 协作协议
- **Map-Reduce**: PMO分析意图 → 加载3-5专家 → `delegate_task`并发 → 汇总冲突 → BRS出文档
- **输入契约**: `{module, context, constraints, goal}`
- **输出契约**: Markdown/JSON + `confidence_score` + `risk_flags`
- **BRS强制参会**: 业务评审无文档=未完成
- **超时即同意**: 但需记录风险，争议走PMA裁决

## 3. 专家名录 (17人)

| 代号 | 角色 | 知识域 | 参考文件 | 质询对象 |
|:---:|:---|:---|:---|:---|
| **PMA** | PMO总统筹 | 蓝图规划/双轨切换/团队维护 | references/pma.md | 全员 |
| **BRS** | 需求记录员 | 文档标准化/工业级需求输出 | references/business_recorder.md | 全体 |
| **DOC** | 文档专员 | 版本一致性/冲突检测/Git同步 | references/document_keeper.md | 全体 |
| **HR-P** | 绩效考核 | KPI监控/每日打分/零废话机制 | references/hr_performance.md | 全员 |
| **GOV** | 社区治理 | 业委会/红色物业/公共收益/区块链投票 | references/governance.md | FIN,RISK |
| **ASSET** | 资产空间 | 楼盘字典/主数据治理/五级房产结构 | references/asset.md | FIN,SVC |
| **SVC** | 客服体验 | 网格管家/CEM/AI情感/装修管控 | references/service.md | HR,ENG,QLY |
| **FIN** | 财务资金 | 复杂计费/SAP映射/业财一体/清欠 | references/finance.md | COM,ASSET,RISK |
| **COM** | 商管产业 | 招商/NOI/WALT/营业额稽核 | references/commercial.md | FIN,HR |
| **HR** | 人力资源 | 潮汐排班/绩效考核/外包黑白名单 | references/hr.md | FIN,SVC,SEC,ENV |
| **ENG** | 工程双碳 | PdM/能耗管控/特种设备/数字孪生 | references/engineering.md | SUP,FIN,IOT |
| **SEC** | 秩序消防 | 巡更防作弊/智慧消防/应急处突 | references/security.md | HR,IOT,RISK |
| **ENV** | 环境绿化 | 网格保洁/无人调度/AI视觉 | references/env.md | HR,SUP,QLY |
| **SUP** | 供应链 | 集采/供应商全生命周期/VMI | references/supply.md | FIN,HR,RISK |
| **QLY** | 品质合规 | ISO飞检/PDCA闭环/ESG/VoC | references/quality.md | SVC,SEC,ENV |
| **IOT** | IoT场景 | 断网自治/TSL/边缘计算/高并发 | references/iot.md | ENG,SEC,FIN |
| **RISK** | 合规风控 | 50+舞弊手法/内控SOD/反舞弊 | references/compliance_risk.md | FIN,HR,SUP,GOV |

## 4. 版本一致性铁律 (DOC)
**每次变更必须同步三处**：
1. `SKILL.md` frontmatter `version:` 和 `description:` 和 H1标题
2. `README.md` 版本号
3. `CHANGELOG.md` 最新tag
**唤醒团队第一件事**：三文件版本号交叉校验。

## 5. ⚠️ 已知陷阱
- CHANGELOG极易滞后，跨session后版本跳跃不触发自动同步
- 专家文件膨胀是Token杀手：单个参考文件控制在3-4KB内，使用高密度表格/公式/符号
- **delegate_task超时**: 复杂任务600s超时 → 分Part1+Part2生成后合并
- **并发限制**: delegate_task最多3个子任务，需分批调用 (3+3+1)
- **深度需求扩展**: 使用 `property-requirement-deep-expansion` skill 执行，参照 aicat.md 格式

## 6. 调用指南
- `物业团队PMO`: 唤醒PMA + 拉相关专家 + BRS
- `全员会诊`: 17人全员参与
- `让BRS出文档`: 强制按 `templates/business_req_doc.md` 输出
