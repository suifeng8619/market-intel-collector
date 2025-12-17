# 方法论优化文档 (Methodology Optimization)

基于 Sprunki 案例的三位情报大师审核意见，系统性优化情报采集与评估方法论。

**版本**: 1.0.0
**创建**: 2025-12-17
**审核者**: 孙子、威廉·斯蒂芬森 (Intrepid)、理查德·佐尔格

---

## 1. 审核专家意见汇总

### 1.1 孙子 (战略层面)

**核心批评**: Battle Index 权重过于静态，未考虑生命周期阶段

| 问题 | 原始方案 | 优化建议 |
|------|----------|----------|
| 权重静态 | 五事等权 | 根据生命周期动态调整 |
| 缺失维度 | 仅评估外部 | 增加"己方能力"评估 |
| 时机判断 | 单一指标 | 多维度时机矩阵 |
| 风险盲区 | 无法律评估 | 增加法律风险维度 |

**建议 Battle Index 调整**:
- 原始计算: 5.1/10
- 调整后: 4.8/10 (因法律风险下调)

### 1.2 威廉·斯蒂芬森 (情报缺口)

**核心批评**: 关键情报缺失，无法支撑决策

| 缺口类型 | 具体问题 | 影响 |
|----------|----------|------|
| 创作者背景 | 仅知年龄，不知身份/历史 | 无法评估持续性 |
| 法律状态 | 无商标/版权信息 | 无法评估IP风险 |
| 竞争情报 | 不知竞品动态 | 无法评估先发优势 |
| 用户画像 | 仅知年龄段 | 无法精准内容策略 |

**情报优先级建议**:
1. P0: 创作者真实身份 + Incredibox官方态度
2. P1: 竞品SEO布局扫描
3. P2: 目标用户平台行为分析

### 1.3 理查德·佐尔格 (战略判断)

**核心批评**: 机会可能是陷阱，建议更保守响应

| 风险因素 | 评估 | 建议 |
|----------|------|------|
| 创作者风险 | 极高 (15岁，已被swat) | 随时可能崩盘 |
| 法律风险 | 高 (fan mod灰色地带) | DMCA随时可能 |
| 时机风险 | 已过峰值86% | 进入衰退期 |
| 投入产出 | L3投入可能打水漂 | 建议L1观望 |

**建议响应等级调整**:
- 原始建议: L3 (谨慎进入)
- 调整建议: L1 (观察) 或 L0 (不行动)

---

## 2. 方法论优化清单

### 2.1 Battle Index 改进

#### 2.1.1 动态权重系统

```yaml
# 根据关键词生命周期阶段调整权重
lifecycle_weights:
  emerging:  # 新兴期 (Trends < 20)
    dao: 0.15
    tian: 0.35  # 时机最重要
    di: 0.25
    jiang: 0.10
    fa: 0.15

  growing:  # 成长期 (Trends 20-60)
    dao: 0.20
    tian: 0.25
    di: 0.25
    jiang: 0.15
    fa: 0.15

  peak:  # 峰值期 (Trends > 60)
    dao: 0.20
    tian: 0.15  # 时机已过，权重下降
    di: 0.30    # 竞争更重要
    jiang: 0.15
    fa: 0.20

  declining:  # 衰退期 (Trends 下降>30%)
    dao: 0.25
    tian: 0.10  # 时机已失
    di: 0.25
    jiang: 0.15
    fa: 0.25    # 产品差异化更重要
```

#### 2.1.2 增加第六事：己 (Self-Assessment)

```yaml
six_factors:
  dao: "用户需求强度"
  tian: "时机窗口"
  di: "竞争态势"
  jiang: "创作者/团队"
  fa: "产品完成度"
  ji: "己方能力"  # 新增

ji_assessment:
  dimensions:
    - domain_expertise: "该领域内容创作能力"
    - resource_available: "可投入资源 (人/时/钱)"
    - response_speed: "响应速度能力"
    - risk_tolerance: "风险承受能力"
    - existing_assets: "已有相关资产 (域名/内容/流量)"

  scoring:
    10: "完全匹配，核心优势领域"
    7-9: "有优势，需少量补充"
    4-6: "能力一般，需较多投入"
    1-3: "能力不足，不建议进入"
```

#### 2.1.3 风险调节系数

```yaml
risk_modifiers:
  legal_risk:
    none: 1.0
    low: 0.95
    medium: 0.85
    high: 0.70      # 高法律风险大幅下调
    critical: 0.50

  creator_risk:
    established: 1.0
    known: 0.95
    anonymous: 0.85
    young_creator: 0.75
    controversial: 0.60

  platform_risk:
    owned_platform: 1.0
    stable_platform: 0.95
    volatile_platform: 0.80
    may_shutdown: 0.60

# 最终 Battle Index = 原始分数 * risk_modifier
# 例: Sprunki = 5.1 * 0.70 (法律) * 0.75 (创作者) = 2.68
```

### 2.2 响应等级优化

#### 2.2.1 增加 L0 等级

```yaml
response_levels:
  L0:  # 新增
    name: "零投入"
    description: "仅记录，不投入任何资源"
    trigger: "Battle Index < 3 或 风险调节后 < 4"
    actions:
      - 记录到案例库
      - 设置自动监控提醒
      - 无任何内容创作
    resource: "0人天"

  L1:
    name: "观察"
    description: "轻度监控，等待更多信号"
    trigger: "3 <= Battle Index < 5"
    actions:
      - 加入监控列表
      - 每周检查趋势变化
      - 准备域名备选清单
    resource: "0.1人天"

  # L2-L5 保持不变...
```

#### 2.2.2 决策时机点

```yaml
decision_gates:
  gate_1:
    timing: "首次发现"
    decision: "Flash Report + 初步 Battle Index"
    threshold: "Index >= 5 进入下一关"

  gate_2:
    timing: "Flash Report 后 24h"
    decision: "情报补充 + 风险评估"
    threshold: "风险调节后 Index >= 4 进入下一关"

  gate_3:
    timing: "Full Report 完成"
    decision: "最终响应等级"
    threshold: "根据综合评估决定 L0-L5"

  abort_conditions:
    - "创作者宣布取消"
    - "收到 DMCA 通知"
    - "Trends 下降 >50%"
    - "竞品大规模进入"
```

### 2.3 情报采集优化

#### 2.3.1 必采情报清单

```yaml
required_intelligence:
  tier_1_critical:  # 必须获取，否则不做决策
    - official_source: "官方网站/页面 URL"
    - creator_identity: "创作者身份 (至少知道是谁)"
    - legal_status: "商标/版权状态"
    - platform_status: "发布平台状态"
    - trends_position: "Google Trends 当前位置"

  tier_2_important:  # 应当获取，影响决策质量
    - creator_history: "创作者过往作品/信誉"
    - competitor_landscape: "主要竞品列表"
    - user_sentiment: "用户正负面评价"
    - ip_holder_stance: "IP持有者态度 (如适用)"
    - monetization_model: "商业模式"

  tier_3_nice_to_have:  # 有则更好，无则可接受
    - detailed_demographics: "详细用户画像"
    - media_coverage: "媒体报道情况"
    - community_size: "社区规模详情"
    - technical_details: "技术实现细节"
```

#### 2.3.2 情报质量评分

```yaml
intelligence_quality:
  score_dimensions:
    completeness: "必采信息的完成度 (0-100%)"
    freshness: "信息新鲜度 (小时内=100, 天内=80, 周内=60, 月内=40)"
    reliability: "来源可靠度 (官方=100, 媒体=80, 社区=60, 推测=20)"
    actionability: "可执行性 (直接可用=100, 需解读=70, 模糊=40)"

  minimum_thresholds:
    for_flash_report: "completeness >= 60%"
    for_full_report: "completeness >= 80%"
    for_l3_response: "completeness >= 85% AND reliability >= 70%"
    for_l4_response: "completeness >= 90% AND reliability >= 80%"
    for_l5_response: "completeness >= 95% AND reliability >= 85%"
```

### 2.4 预测模型优化

#### 2.4.1 生命周期预测

```yaml
lifecycle_prediction:
  models:
    explosive_decay:
      characteristics:
        - "初期增长 >500%/周"
        - "社交媒体驱动"
        - "缺乏持续内容更新"
      typical_half_life: "2-4周"
      examples: ["viral memes", "one-hit games"]

    gradual_growth:
      characteristics:
        - "稳定增长 20-100%/月"
        - "产品驱动"
        - "有持续开发"
      typical_lifespan: "6-24个月"
      examples: ["quality indie games", "useful tools"]

    evergreen:
      characteristics:
        - "增长后稳定"
        - "解决持续性需求"
        - "有生态系统"
      typical_lifespan: "3-10年+"
      examples: ["Minecraft", "Notion"]

    revival_potential:
      characteristics:
        - "曾经热门后衰退"
        - "有忠实用户群"
        - "可能有续作/更新"
      revival_triggers: ["续作发布", "平台移植", "主播带动"]
```

#### 2.4.2 竞争窗口预测

```yaml
competition_window:
  calculation:
    inputs:
      - current_serp_quality: "当前SERP内容质量 (1-10)"
      - keyword_difficulty: "关键词难度 (1-100)"
      - trend_velocity: "趋势增长速度"
      - big_player_signals: "大玩家进入信号"

    formula: |
      base_window = 30 * (10 - serp_quality) / 10  # 天
      velocity_modifier = 1 / (1 + trend_velocity/100)
      big_player_modifier = 0.5 if big_player_signals else 1.0
      window = base_window * velocity_modifier * big_player_modifier

  alerts:
    - "window < 7天: 紧急，需立即决策"
    - "window 7-14天: 高优先级"
    - "window 14-30天: 正常处理"
    - "window > 30天: 可观望"
```

---

## 3. 案例回溯：Sprunki 优化评估

### 3.1 原始评估 vs 优化评估

| 维度 | 原始分数 | 优化分数 | 变化原因 |
|------|----------|----------|----------|
| 道 (User Pain) | 7/10 | 7/10 | 不变 |
| 天 (Timing) | 2/10 | 2/10 | 不变 |
| 地 (Competition) | 5/10 | 5/10 | 不变 |
| 将 (Team) | 4/10 | 3/10 | 创作者已退出 |
| 法 (Product) | 8/10 | 7/10 | 无法确认后续支持 |
| **己 (Self)** | - | 5/10 | 新增：一般能力 |
| **原始 Index** | 5.1/10 | 4.8/10 | |
| **风险调节** | - | ×0.70×0.75 | 法律+创作者风险 |
| **最终 Index** | 5.1/10 | **2.5/10** | 大幅下调 |

### 3.2 优化后响应建议

| 原始建议 | 优化建议 | 理由 |
|----------|----------|------|
| L3 (谨慎进入) | **L0 (零投入)** | 风险调节后 Index=2.5 < 3 |
| 投入5人天 | 投入0人天 | 不符合最低阈值 |
| 域名检查 | 仅记录案例 | 保存学习价值 |

### 3.3 如果在 2024-09 评估 (假设)

| 维度 | 2024-09评估 | 说明 |
|------|-------------|------|
| 道 | 9/10 | 需求旺盛 |
| 天 | 10/10 | 最佳时机 |
| 地 | 9/10 | 竞争空白 |
| 将 | 4/10 | 年轻创作者风险 |
| 法 | 8/10 | 产品完成度高 |
| 己 | 6/10 | 有能力但非核心领域 |
| **原始 Index** | 7.7/10 | 高机会 |
| **风险调节** | ×0.70×0.85 | 法律+创作者(当时未暴露) |
| **最终 Index** | **4.6/10** | 边缘决策 |
| **建议** | L2 (准备) | 观望+准备，等待更多信号 |

**教训**: 即使在最佳时机，风险评估也会将响应级别下调。这避免了后来的损失。

---

## 4. 实施检查清单

### 4.1 短期优化 (本周)

- [x] data-sources.md 增加法律/竞争情报源
- [x] hotspot-monitors.md 增加情报专用哨站
- [x] 创建本文档记录方法论改进
- [ ] 更新 SKILL.md 引用新方法论
- [ ] 更新 schema.md 增加六事评估格式

### 4.2 中期优化 (本月)

- [ ] 实现动态权重计算逻辑
- [ ] 创建风险调节系数参考表
- [ ] 设计情报质量评分卡
- [ ] 建立 L0 响应等级流程

### 4.3 长期优化 (持续)

- [ ] 积累案例库，校准预测模型
- [ ] 每月评估哨站效能，调整阈值
- [ ] 每季度审视方法论，迭代优化
- [ ] 建立自动化情报质量检查

---

## 5. 方法论版本历史

| 版本 | 日期 | 变更 |
|------|------|------|
| 1.0.0 | 2025-12-17 | 初版：基于三位专家审核创建 |

---

*Methodology optimization reference for market-intel-collector skill*
*Reviewed by: Sun Tzu, William Stephenson, Richard Sorge*
*Version: 1.0.0*
