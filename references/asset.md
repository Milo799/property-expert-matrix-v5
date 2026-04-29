# 资产空间专家参考 | Asset Space Expert Reference v5
## ① 四级房产资源结构 🏗️ 行业标准 + 元数据扩展
| 层级 | 标识 | 粒度 | 行业适配示例 |
|------|------|------|----------|
| L1 项目 Project | `PR-{code}` | 独立产权/物理边界 | 住宅社区/写字楼/商业综合体 |
| L2 楼栋 Building | `BL-{pr}-{code}` | 单体建筑/期数/苑区 | A栋/1期/东区/商业裙楼 |
| L3 单元/楼层 Unit | `UT-{bl}-{code}` | 垂直分段/水平分区 | 单元/楼层/功能区 |
| L4 房间 Room | `RM-{ut}-{code}` | 最小使用单元 | 住宅户/商铺/车位/机房 |
> 📐 **编码**: `{L1}{L2}{L3}{L4}` 级联, `parent_id` 双向导航
> 🧩 **扩展**: `metadata` JSONB 存储 `zone/district/phase/type` 等自定义维度，不增加固定层级

## ② 一户一档全景视图 🗂️ 12数据域实时聚合
①权属信息 ②物理属性 ③合同档案 ④财务账单 ⑤能耗计量 ⑥维修记录 ⑦装修备案 ⑧租户画像 ⑨合规证照 ⑩安防设备 ⑪社区活动 ⑫投诉工单
```
聚合引擎: CQRS + EventSourcing → RoomView{domains: [D1..D12], version: ulong, ts: now}
```
## ③ 房产状态机 🔄 9阶段生命周期
```
[在建]──竣工备案──→[预售]──交付通知──→[交付]──钥匙交接──→[入住]
                                                        ↓
[报废]←──鉴定报废←──[空置](90d+)←──退租结算←──[退住]←──退住申请←──[正常]
                          ↓                          ↓
                       [装修]←──装修审批←──装修申请←──[入住]
状态转换约束: 仅允许相邻状态跳转, 跨级需审批流; 每步产生 AuditLog{from,to,actor,ts,reason}
```

## ④ 共有产权管理 ⚖️
| 业主 | 份额比例 pᵢ | 表决权权重 wᵢ = pᵢ × f(pᵢ) | 决策规则 |
|------|------------|---------------------------|----------|
| A | 50% | 0.50×1.0 = 0.50 | 重大事项 ≥2/3 表决权 |
| B | 30% | 0.30×1.0 = 0.30 | 一般事项 ≥1/2 表决权 |
| C | 20% | 0.20×1.0 = 0.20 | 一票否决: pᵢ≥50%时生效 |
> 表决权修正: f(pᵢ) = 1 + λ·log(1+pᵢ), λ∈[0,0.1]; 总份额 Σpᵢ = 100%

## ⑤ 客户合并与拆分 🔀
**相似度算法**: `Sim(A,B) = α·Jaccard(name) + β·Levenshtein(phone) + γ·ExactMatch(idCard)`
| 规则 | 阈值 | 动作 |
|------|------|------|
| Sim ≥ 0.95 | 自动合并 | 保留最新字段+合并关联资产 |
| 0.80 ≤ Sim < 0.95 | 人工审核 | 展示Diff+冲突标记 |
| Sim < 0.80 | 跳过 | 无操作 |
**冲突检测**: 字段值不同 → 优先级: 官方证件 > 合同 > 系统录入 > 历史快照
**拆分规则**: 合并后30d内可回滚; 拆分产生 SplitEvent{src, dst[], audit[]}

## ⑥ 数据校验规则 ✅ 24项必填 + 8条业务规则
**必填字段**: `id, project_code, bldg_code, unit_code, room_code, floor, area_usable, area_build, orientation, room_type, status, owner_id, contract_id, price_unit, price_total, pay_method, energy_meter, fire_device, entry_date, last_update, created_by, tags[], geo_point, metadata{}`
| # | 规则 | 表达式 | 级别 |
|---|------|--------|------|
| R01 | 面积正数 | area_usable > 0 ∧ area_build ≥ area_usable | ERROR |
| R02 | 楼层合理 | 1 ≤ floor ≤ bldg.total_floors | ERROR |
| R03 | 价格区间 | 0 < price_unit ≤ 500000 | WARN |
| R04 | 状态一致性 | status ∈ 状态机合法值 | ERROR |
| R05 | 编码级联 | PR→BL→UT→RM 父链存在 | ERROR |
| R06 | 产权约束 | Σownership_pct = 100% | ERROR |
| R07 | 日期逻辑 | entry_date ≤ today ∧ last_update ≥ created_at | ERROR |
| R08 | 能耗上限 | monthly_energy ≤ area_build × threshold_kwh | WARN |