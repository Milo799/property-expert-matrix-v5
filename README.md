# 智慧物业专家矩阵 V7.2.1

> 全场景业务精通 / 合规红线 / 质询完善 / 知识卡片化

## 版本信息

| 项目 | 值 |
|------|-----|
| **版本** | V7.2.1 |
| **作者** | PMA (PMO总统筹) |
| **许可证** | MIT |
| **技能路径** | `~/.hermes/skills/property-expert-matrix-v5/` |
| **GitHub** | `Milo799/property-expert-matrix-v5` |

## 核心理念

**按需加载 · 深度优先 · Token极简**。每位专家浓缩为"知识卡片"：高密度结构化数据，零废话，只保留决策必需的框架/公式/规则。

## 专家矩阵 (17人)

| 代号 | 角色 | 知识域 |
|:---:|:---|:---|
| **PMA** | PMO总统筹 | 蓝图规划/双轨切换/接管验收/多业态统筹 |
| **BRS** | 需求记录员 | 文档标准化/RTM追溯/WSJF优先级/变更管理 |
| **DOC** | 文档专员 | 版本一致性/冲突检测/Git同步/依赖图谱 |
| **HR-P** | 绩效考核 | KPI监控/每日打分/零废话/红黄牌/PIP |
| **GOV** | 社区治理 | 业委会/红色物业/公共收益/电子投票存证 |
| **ASSET** | 资产空间 | 四级结构/主数据治理/一户一档/楼层独立 |
| **SVC** | 客服体验 | 网格管家/CEM/AI情感/装修管控/客户关怀 |
| **FIN** | 财务资金 | 复杂计费/民法典公共收益/SAP映射/业财一体 |
| **COM** | 商管产业 | 招商/NOI/营业额稽核/民法典租赁法务 |
| **HR** | 人力资源 | 潮汐排班/派遣<10%红线/外包管理/薪酬福利 |
| **ENG** | 工程双碳 | PdM/特种设备15天维保/能耗管控/碳核算 |
| **SEC** | 秩序消防 | 巡更防作弊/消防法第一责任人/应急处突 |
| **ENV** | 环境绿化 | 网格保洁/消杀/环保法合规台账/外包考核 |
| **SUP** | 供应链 | 集采/SLA外包考核/零库存/工程备件 |
| **QLY** | 品质合规 | ISO飞检/PDCA闭环/VoC/ESG/SPC统计 |
| **IOT** | IoT场景 | 智慧通行断网自治/消防联动/边缘计算 |
| **RISK** | 合规风控 | 50+舞弊手法/SOD/Benford/等保2.0 |

## 协作协议

- **Map-Reduce**: PMO分析意图 → 加载3-5专家 → `delegate_task`并发 → 汇总冲突 → BRS出文档
- **输入契约**: `{module, context, constraints, goal}`
- **输出契约**: Markdown/JSON + `confidence_score` + `risk_flags`
- **BRS强制参会**: 业务评审无文档=未完成
- **超时即同意**: 但需记录风险，争议走PMA裁决

## 版本一致性铁律

每次变更必须同步三处：
1. `SKILL.md` frontmatter `version:` 和 `description:` 和 H1标题
2. `README.md` 版本号
3. `CHANGELOG.md` 最新tag

## 目录结构

```
property-expert-matrix-v5/
├── SKILL.md              # 主技能文件
├── README.md             # 本文件
├── CHANGELOG.md          # 版本变更记录
├── references/           # 17位专家知识卡片
│   ├── pma.md
│   ├── business_recorder.md
│   ├── document_keeper.md
│   ├── hr_performance.md
│   ├── governance.md
│   ├── asset.md
│   ├── service.md
│   ├── finance.md
│   ├── commercial.md
│   ├── hr.md
│   ├── engineering.md
│   ├── security.md
│   ├── env.md
│   ├── supply.md
│   ├── quality.md
│   ├── iot.md
│   └── compliance_risk.md
└── templates/            # 文档模板
    ├── roundtable_minutes.md
    ├── expert_deep_dive.md
    ├── business_req_doc.md
    └── meeting_minutes.md
```

## 使用方式

- `物业团队PMO`: 唤醒PMA + 拉相关专家 + BRS
- `全员会诊`: 17人全员参与
- `让BRS出文档`: 强制按 `templates/business_req_doc.md` 输出

---
*V7.2 | 17位专家 | 知识卡片化 | 全场景覆盖*
