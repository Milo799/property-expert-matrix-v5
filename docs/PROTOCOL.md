# 🤖 专家间通信协议 (V7.2: 全场景/合规红线/质询完善版)

## 1. 动态按需加载与质询协议 (Map-Inquire-Reduce)
PMO **禁止**一次性唤醒全员。必须执行 **Map-Inquire-Reduce** 流程：
- **Step 1 (Map)**: PMO 分析需求 -> 加载 3-5 个核心专家 (仅读取相关 Prompt)。
- **Step 1.5 (Inquire - New V7.2)**: 核心专家输出后，PMO 根据质询矩阵拉起**质询对象** (如 SVC 提出账单争议 -> 必须拉起 FIN 回应)。
- **Step 2 (Concurrent)**: `delegate_task` 并发执行。Context 仅含任务背景 + 自身 Prompt + 质询输入。
- **Step 3 (Reduce)**: PMO 汇总 -> DOC 冲突检测 -> BRS 汇编。

## 2. 依赖关系矩阵 (Dependency Matrix for DOC)
DOC 执行增量冲突检测时，必须检查以下关联文件 (**V7.2 Update**):

| 变更文件 (Target) | 必须检查的依赖文件 (Dependencies) | V7.2 新增质询关注点 |
| :--- | :--- | :--- |
| `finance.md` | `commercial.md`, `asset.md`, `governance.md`, `compliance_risk.md` | 公共收益分账/计费与租赁对齐 |
| `commercial.md` | `finance.md`, `hr.md`, `asset.md`, `compliance_risk.md` | 租赁合同法务/铺位状态 |
| `asset.md` | `finance.md`, `commercial.md`, `service.md`, `engineering.md` | 房产结构变更/楼层属性/设备位置 |
| `service.md` | `hr.md`, `engineering.md`, `quality.md`, `asset.md`, `finance.md` | 报修关联资产/账单争议 |
| `engineering.md` | `iot.md`, `asset.md`, `supply.md`, `security.md` | 设备位置变更/特种设备合规 |
| `security.md` | `iot.md`, `engineering.md`, `hr.md`, `compliance_risk.md` | 消防联动/排班/第一责任人 |
| `iot.md` | `engineering.md`, `security.md`, `env.md` | 消防联动/断网自治 |
| `governance.md` | `asset.md`, `finance.md`, `compliance_risk.md` | 业委会用房权属/公共收益 |
| `quality.md` | `service.md`, `env.md`, `security.md`, `compliance_risk.md` | 品质整改闭环 |
| `supply.md` | `engineering.md`, `env.md`, `finance.md`, `hr.md` | 备件采购/SLA 考核 |
| `hr.md` | `finance.md`, `security.md`, `supply.md` | 派遣红线/外包合规 |
| `compliance_risk.md` | **ALL** (全局扫描) | 50+ 舞弊手法匹配 |

## 3. PMO 状态机 (State Machine)
PMO 必须在 Context 中维护 Checklist，**强制包含合规检查**:
```json
{
  "workflow_state": {
    "step_1_load_experts": true,
    "step_1_5_inquiry": "Pending/Complete",
    "step_2_parallel_execution": true,
    "step_3_compliance_check": true, 
    "step_4_brs_synthesis": false,
    "step_5_doc_sync": false
  }
}
```

## 4. BRS 汇编协议 (Synthesis Protocol)
BRS 汇编需求文档时，必须增加 **“合规性声明” (Compliance Statement)** 章节：
- **Input**: 专家 A 观点 + 专家 B 质询 + PMA 裁决 + **法律法规引用** (如《民法典》282 条)。
- **Output**: 结构化 Markdown，必须包含风险标记 (Risk Flags) 和置信度 (Confidence Score)。

## 5. DOC 增量同步协议
- **Trigger**: PMO 告知“变更了哪些文件”。
- **Action**: 
  1. 读取依赖矩阵 -> 检查冲突。
  2. **V7.2 强制检查**: 检查 `asset.md` 是否为“四级结构”；检查 `finance.md` 是否包含“公共收益”；检查 `hr.md` 是否包含“派遣红线”。
  3. 更新文档 -> Git Push。