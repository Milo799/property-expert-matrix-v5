# IoT 场景专家参考卡片 · v5
## 1. IoT平台5层架构
|层级|职责|技术栈|延迟|
|---|---|---|---|
|L1感知|采集/执行|传感器·执行器·RFID|μs~ms|
|L2网络|传输/路由|WiFi/Zigbee/LoRa/NB-IoT/5G|ms~s|
|L3边缘|本地计算/决策|网关·规则引擎·轻量AI|ms|
|L4平台|设备管理/数据|消息总线·时序库·TSL|ms~s|
|L5应用|业务逻辑/展示|数字孪生·告警·大屏|s|
> 原则: L3自治≥70% | L4≥99.95% | L5可降级
## 2. 断网自治Failover
`Failover=P(断网)×[L3缓存(≤72h)+本地规则+延迟同步]`
|阶段|动作|目标|
|---|---|---|
|检测|心跳≥3次·Keepalive·DNS|≤10s识别|
|切换|本地队列↑·规则接管·离线策略|切换≤2s|
|自治|定时任务·本地告警·缓存数据|RPO=0|
|恢复|增量同步(Δ序列)·冲突(LWW)|RTO≤60s|
> MTTR≤60s | 丢数据<0.01% | 脑裂=Quorum+Lease
## 3. 设备接入协议
|协议|传输|特性|场景|负载|
|---|---|---|---|---|
|MQTT 5.0|TCP|QoS0/1/2·Pub/Sub·遗嘱|遥测/命令|~256KB|
|CoAP|UDP|RESTful·Observe·DTLS|低功耗传感|~1.5KB|
|Modbus|TCP/串口|主从·FC01~16·轮询|PLC/工控|≤256B|
|BACnet/IP|UDP|对象模型·BBMD|楼宇自控|≤1.5KB|
> Score=w₁·功耗+w₂·延迟+w₃·可靠 → argmax
## 4. TSL物模型标准化
`Thing={props[]+svcs[]+evts[]}` | `prop:{id,type,unit,access,spec{min,max,enum}}` | `svc:{id,input[],output[]}` | `evt:{id,type(info/alert/fatal),level}`
|步骤|动作|示例|
|---|---|---|
|①分类|行业域(空调/电梯/门禁)|`AC-2024-v3`|
|②建模|属性/服务/事件三元组|`temp:float°C`|
|③约束|值域·单位·权限|`r/w:16~32°C`|
|④注册|Schema Registry版本|GitOps审核|
> 路径: 物模型→协议适配器→统一抽象层
## 5. 高并发削峰
`Peak=QPS_max/QPS_avg → Peak>5 触发` | `削峰=MQ缓冲+批量聚合+异步写入+降采样`
|策略|机制|效果|
|---|---|---|
|消息队列|Kafka分区·堆积预警|吞吐×10|
|批量聚合|窗口(1s/10条)→批量写|IO↓70%|
|异步写入|WAL→内存→刷盘|延迟↓90%|
|降采样|指数/线性/极值保留|存储↓60%|
|限流熔断|Token Bucket+熔断器|保护下游|
> 容量: C=(QPS×payload×retention)/compression
## 6. 智能设备管理
|阶段|操作|自动化|
|---|---|---|
|注册|证书·影子|Zero-Touch Provision|
|在线|心跳·OTA·配置|策略驱动·灰度|
|异常|离线·诊断·重连|自愈脚本|
|退役|归档·撤销·清理|生命周期流转|
## 7. 数据安全与隐私
|维度|控制措施|合规标准|
|---|---|---|
|传输|TLS1.3/DTLS·mTLS·证书轮换|IEC 62443|
|存储|AES-256·KMS·字段加密|GDPR·等保2.0|
|访问|RBAC/ABAC·最小权限·临时Token|OAuth2.0/OIDC|
|隐私|脱敏·本地化·最小采集·同意|PIPL·CCPA|
|审计|不可篡改日志·SIEM联动·合规报告|SOC2|
> Risk=Σ(Threat×Vuln×Impact)/Control_Effective
## 8. IoT与业务联动
`链路: IoT→Event→Rule→Action→业务系统→反馈闭环`
|场景|触发条件|联动动作|价值|
|---|---|---|---|
|节能|无人∧T>26°C≥30min|降温+关窗|能耗↓20%|
|维保|振动异常∧时长阈值|工单+备件锁定|停机↓40%|
|安防|门禁异常+AI告警|调度+录像锁定|响应<3min|
|客服|设备故障→推送|智能客服介入|满意度↑|
> ΔRevenue=IoT自动化×转化率×ARPU