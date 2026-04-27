# 🤖 专家间通信协议 (V6.1: Map-Reduce 架构)

## 1. 动态按需加载协议 (Dynamic Loading Protocol)
PMO **禁止**一次性唤醒全员。必须根据任务意图执行 **Map-Reduce** 流程：
- **Step 1 (Map)**: PMO 分析需求 -> 仅读取相关专家 Prompt (3-5 人)。
- **Step 2 (Concurrent)**: 使用 `delegate_task` 并发调用专家 (Context 仅包含任务背景 + 自身 Prompt)。
- **Step 3 (Reduce)**: PMO 收集结果 -> 识别冲突 -> 单点质询 -> 最终裁决。

## 2. 依赖关系矩阵 (Dependency Matrix for DOC)
DOC 执行增量冲突检测时，仅需检查以下关联文件：

| 变更文件 (Target) | 必须检查的依赖文件 (Dependencies) |
| :--- | :--- |
| `finance.md` | `commercial.md`, `asset.md`, `compliance_risk.md`, `SKILL.md`, `README.md` |
| `commercial.md` | `finance.md`, `hr.md`, `compliance_risk.md`, `SKILL.md`, `README.md` |
| `service.md` | `hr.md`, `engineering.md`, `quality.md`, `compliance_risk.md`, `SKILL.md` |
| `hr.md` | `service.md`, `engineering.md`, `security.md`, `supply.md`, `finance.md` |
| `iot.md` | `engineering.md`, `security.md`, `env.md`, `SKILL.md` |
| `engineering.md` | `iot.md`, `finance.md`, `env.md`, `supply.md` |
| `security.md` | `iot.md`, `env.md`, `hr.md`, `compliance_risk.md` |
| `env.md` | `engineering.md`, `security.md`, `hr.md`, `supply.md` |
| `supply.md` | `engineering.md`, `env.md`, `finance.md`, `hr.md` |
| `quality.md` | `service.md`, `env.md`, `security.md`, `compliance_risk.md` |
| `asset.md` | `finance.md`, `commercial.md`, `service.md`, `compliance_risk.md` |
| `governance.md` | `service.md`, `compliance_risk.md` |
| `compliance_risk.md` | **ALL** (全局扫描) |
| `document_keeper.md` | `SKILL.md`, `README.md` |

## 3. PMO 状态机 (State Machine)
PMO 必须在 Context 中维护 Checklist，杜绝流程跳跃：
```json
{
  "workflow_state": {
    "step_1_load_experts": true,
    "step_2_parallel_execution": true,
    "step_3_conflict_check": true,
    "step_4_brs_synthesis": false,
    "step_5_doc_sync": false
  }
}
```

## 4. BRS 汇编协议 (Synthesis Protocol)
BRS **不再旁听**，而是基于 PMO 提供的**结构化输入**进行汇编：
- **Input**: 用户故事 + 专家 A 观点 + 专家 B 观点 + PMA 裁决。
- **Action**: 生成《业务需求说明书》，并在“讨论记录”章节引用输入数据。

## 5. DOC 增量同步协议
- **Trigger**: PMO 告知“变更了哪些文件”。
- **Action**: 读取依赖矩阵 -> 仅检查相关文件的冲突 -> 更新文档 -> Git Push。