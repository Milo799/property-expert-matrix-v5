# 📋 文档专员 · 知识卡片 v5

## ▸ 全局冲突检测引擎
| 冲突类型      | 检测规则                          | 处置动作          |
|---------------|-----------------------------------|-------------------|
| 内容冲突      | `H(docA,docB) > θ_sim`            | 标记冲突区 + 仲裁 |
| 元数据冲突    | `schema_vA ≠ schema_vB`           | 升版至最新 schema  |
| 结构冲突      | `TOC_A ⊄ TOC_B ∧ TOC_B ⊄ TOC_A`   | 合并 TOC 树       |
> 冲突公式: `C = Σ(wᵢ·simᵢ) / Σwᵢ`, θ_sim=0.85

## ▸ 版本一致性校验
- **哈希锚定**: `SHA256(doc) → 基线指纹`, 变更时重算校验
- **语义版本**: `MAJOR.MINOR.PATCH` — BREAKING/MINOR/FIX
- **一致性矩阵**: `∀d∈Docs, Consistent(d) ⇔ hash(d) == registry[d]`
- **漂移告警**: 基线偏离 ≥3 次 → 触发全量校验

## ▸ Git同步自动化
```
sync_cycle: fetch → rebase → lint → commit → push
commit_msg: [docs]{scope}: {desc} (via doc_keeper)
conflict_strategy: ours(base) + theirs(content) → merge_commit
branch_policy: feature/* → staging → main (PR + review gate)
```

## ▸ 文档模板管理
| 模板类别    | 关键字段                        | 校验规则              |
|-------------|---------------------------------|-----------------------|
| API文档     | endpoint, method, request, response | schema必填          |
| 操作手册    | step, condition, expected       | 编号连续+可执行验证   |
| 设计文档    | context, decision, consequence  | ADR格式合规           |
> 模板引擎: Jinja2 渲染 + JSON Schema 校验

## ▸ 依赖关系图谱
- **节点**: `V={文档, 模块, 接口}`, **边**: `E={(u,v)| u refs v}`
- **有向图**: `G=(V,E)`, 用拓扑排序检测循环依赖
- **传播规则**: `dep(u) → dep(v) ⇒ Δu ⇒ 重新评估 v`
- **可视化**: Mermaid graph TD + DOT 导出

## ▸ 文档质量评分模型
`Q = w₁·Completeness + w₂·Accuracy + w₃·Freshness + w₄·Readability`
| 维度          | 权重  | 计算方式                      |
|---------------|-------|-------------------------------|
| 完整性        | 0.30  | 必填字段覆盖率                |
| 准确性        | 0.30  | 代码示例编译通过率            |
| 时效性        | 0.25  | `exp(-λ·Δt)`, λ=0.1/月       |
| 可读性        | 0.15  | Flesch-Kincaid / 中文分句密度  |
> 阈值: Q≥0.80 发布 | 0.60-0.80 修订 | <0.60 重构

## ▸ 变更影响分析
- **直接影响**: `I_direct = {d | d ∈ changed_docs}`
- **级联影响**: `I_cascade = {d | ∃ path(d→dᵢ) in G, dᵢ∈I_direct}`
- **影响等级**: 🔴 HIGH(核心API) 🟡 MED(内部模块) 🟢 LOW(辅助说明)
- **输出**: 影响报告 + 待更新清单 + 自动化 PR 生成

## ▸ 归档与检索策略
| 生命周期阶段 | 保留策略                | 存储层级  |
|--------------|-------------------------|-----------|
| 活跃(≤6月)   | 全量索引+实时搜索       | Hot/SSD   |
| 历史(6-24月) | 压缩索引+标签检索       | Warm/HDD  |
| 归档(>24月)  | 快照存储+元数据保留     | Cold/S3   |
> 检索: BM25 + 向量嵌入混合搜索 | 标签: `status:deprecated` 隔离过期文档

---
⚡ v5 | Tokens: ~650 | Cards: 8 | 状态: 就绪
