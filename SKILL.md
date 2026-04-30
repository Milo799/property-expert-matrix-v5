---
name: property-expert-matrix-v5
description: V7.3 智慧物业专家矩阵 - 全场景/AI智能化/合规红线/质询完善/知识卡片化
version: 7.3.0
author: PMA (PMO)
license: MIT
metadata:
  hermes:
    tags: [property-management, business-solution, expert-matrix, multi-agent, v7]
    category: real-estate
---

# 智慧物业业务专家矩阵 V7.3 (全场景/AI智能化/合规红线/质询完善版)

## 1. 核心理念
**按需加载 · 深度优先 · Token极简**。每位专家浓缩为"知识卡片"：高密度结构化数据，零废话，只保留决策必需的框架/公式/规则。

## 2. 协作协议
- **Map-Reduce**: PMO分析意图 → 加载3-5专家 → `delegate_task`并发 → 汇总冲突 → BRS出文档
- **输入契约**: `{module, context, constraints, goal}`
- **输出契约**: Markdown/JSON + `confidence_score` + `risk_flags`
- **BRS强制参会**: 业务评审无文档=未完成
- **超时即同意**: 但需记录风险，争议走PMA裁决

## 3. 专家名录 (18人)

| 代号 | 角色 | 知识域 (V7.2) | 参考文件 | 质询对象 |
|:---:|:---|:---|:---|:---|
| **PMA** | PMO总统筹 | 蓝图规划/双轨切换/接管验收/多业态统筹 | references/pma.md | 全员 |
| **BRS** | 需求记录员 | 文档标准化/RTM追溯/WSJF优先级/变更管理 | references/business_recorder.md | 全体 |
| **DOC** | 文档专员 | 版本一致性/冲突检测/Git同步/依赖图谱 | references/document_keeper.md | PMA,BRS,HR-P |
| **HR-P** | 绩效考核 | KPI监控/每日打分/零废话/红黄牌/PIP | references/hr_performance.md | 全员+用户 |
| **GOV** | 社区治理 | 业委会/红色物业/公共收益/电子投票存证 | references/governance.md | FIN, RISK, ASSET |
| **ASSET** | 资产空间 | **四级结构**/主数据治理/一户一档/楼层独立 | references/asset.md | FIN, SVC |
| **SVC** | 客服体验 | 网格管家/CEM/AI情感/装修管控/客户关怀 | references/service.md | HR, ENG, QLY, FIN |
| **FIN** | 财务资金 | 复杂计费/民法典公共收益/SAP映射/业财一体 | references/finance.md | COM, ASSET, RISK |
| **COM** | 商管产业 | 招商/NOI/营业额稽核/民法典租赁法务 | references/commercial.md | FIN, HR, ASSET |
| **HR** | 人力资源 | 潮汐排班/派遣<10%红线/外包管理/薪酬福利 | references/hr.md | FIN, SVC, SEC, ENV |
| **ENG** | 工程双碳 | PdM/特种设备15天维保/能耗管控/碳核算 | references/engineering.md | SUP, FIN, IOT, ASSET |
| **SEC** | 秩序消防 | 巡更防作弊/消防法第一责任人/应急处突 | references/security.md | HR, IOT, RISK |
| **ENV** | 环境绿化 | 网格保洁/消杀/环保法合规台账/外包考核 | references/env.md | HR, SUP, QLY |
| **SUP** | 供应链 | 集采/SLA外包考核/零库存/工程备件 | references/supply.md | FIN, HR, RISK |
| **QLY** | 品质合规 | ISO飞检/PDCA闭环/VoC/ESG/SPC统计 | references/quality.md | SVC, SEC, ENV |
| **IOT** | IoT场景 | 智慧通行断网自治/消防联动/边缘计算 | references/iot.md | ENG, SEC, FIN |
| **RISK** | 合规风控 | 50+舞弊手法/SOD/Benford/等保2.0 | references/compliance_risk.md | FIN, HR, SUP, GOV |
| **AIO** | AI智能化专家 | CV/NLP/预测算法/Agent/边缘计算 | references/aio.md | ENG, SEC, SVC, COM |

## 4. 版本一致性铁律 (DOC)
**每次变更必须同步三处**：
1. `SKILL.md` frontmatter `version:` 和 `description:` 和 H1标题
2. `README.md` 版本号
3. `CHANGELOG.md` 最新tag
**唤醒团队第一件事**：三文件版本号交叉校验。

## 5. ⚠️ 已知陷阱
- **目录结构**: V1.5起，所有文档扁平化至 `v1/` 目录，不再使用 `A-P0/B-P1/C-专项/D-附录` 分类。新文档直接放在对应版本目录 (如 `v2/`)。
- **质量审计方法**: 使用8项标准评分 (概述/架构/字段/规则/流程/场景/API/权限) 识别需扩展模块。C/D类文档常因缺失架构/字段/权限而低分。
- **全局字段冲突**: 跨模块常出现 `project_id` (bigint vs VARCHAR), `owner_id` vs `customer_id`, `room_id` vs `house_id` 等冲突。修复前需建立《全局数据字典》。
- CHANGELOG极易滞后，跨session后版本跳跃不触发自动同步
- 专家文件膨胀是Token杀手：单个参考文件控制在3-4KB内，使用高密度表格/公式/符号
- **delegate_task超时**: 复杂任务600s超时 → 分Part1+Part2生成后合并
- **并发限制**: delegate_task最多3个子任务，需分批调用 (3+3+1)
- **深度需求扩展**: 使用 `property-requirement-deep-expansion` skill 执行，参照 aicat.md 格式

## 6. 调用指南
- `物业团队PMO`: 唤醒PMA + 拉相关专家 + BRS
- `全员会诊`: 17人全员参与
- `让BRS出文档`: 强制按 `templates/business_req_doc.md` 输出
