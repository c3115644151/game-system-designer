# 数值框架Excel表格结构说明

数值设计的Excel/Spreadsheet部分，用于配置表、模拟计算和数据验证。

## 表格类型

### 1. 经济系统配置表

#### 1.1 货币产出表（Source）
| 字段名 | 类型 | 说明 | 示例值 |
|-------|------|------|-------|
| source_id | int | 产出来源ID | 1001 |
| source_name | string | 产出来源名称 | "日常任务" |
| currency_type | string | 货币类型 | "gold"/"diamond" |
| amount | int | 产出数量 | 100 |
| frequency | string | 产出频率 | "daily"/"per_completion" |
| condition | string | 产出条件 | "完成所有日常任务" |

#### 1.2 货币消耗表（Sink）
| 字段名 | 类型 | 说明 | 示例值 |
|-------|------|------|-------|
| sink_id | int | 消耗去向ID | 2001 |
| sink_name | string | 消耗去向名称 | "抽卡" |
| currency_type | string | 货币类型 | "gold"/"diamond" |
| amount | int | 消耗数量 | 280 |
| frequency | string | 消耗频率 | "per_pull" |
| condition | string | 消耗条件 | "单次抽卡" |

#### 1.3 兑换比率表
| 字段名 | 类型 | 说明 | 示例值 |
|-------|------|------|-------|
| from_currency | string | 源货币 | "gold" |
| to_currency | string | 目标货币 | "diamond" |
| exchange_rate | float | 兑换比率 | 0.01 |
| min_amount | int | 最小兑换量 | 100 |
| max_amount | int | 最大兑换量 | 10000 |
| daily_limit | int | 每日限制 | 1000 |

#### 1.4 经济平衡计算表
| 字段名 | 类型 | 说明 | 示例值 |
|-------|------|------|-------|
| metric_name | string | 指标名称 | "日产出总量" |
| currency_type | string | 货币类型 | "gold" |
| calculated_value | float | 计算值 | 1500 |
| target_value | float | 目标值 | 1250 |
| ratio | float | 产出/消耗比 | 1.2 |
| status | string | 状态 | "正常"/"偏高"/"偏低" |

---

### 2. 成长系统配置表

#### 2.1 等级经验表
| 字段名 | 类型 | 说明 | 示例值 |
|-------|------|------|-------|
| level | int | 等级 | 1 |
| exp_required | int | 本级所需经验 | 100 |
| exp_cumulative | int | 累计经验 | 100 |
| exp_formula | string | 经验公式 | "100 * level^1.5" |

#### 2.2 属性成长表
| 字段名 | 类型 | 说明 | 示例值 |
|-------|------|------|-------|
| level | int | 等级 | 1 |
| stat_hp | int | 生命值 | 1000 |
| stat_atk | int | 攻击力 | 100 |
| stat_def | int | 防御力 | 50 |
| stat_hp_growth | float | 生命成长系数 | 1.1 |
| stat_atk_growth | float | 攻击成长系数 | 1.05 |

#### 2.3 成长曲线验证表
| 字段名 | 类型 | 说明 | 示例值 |
|-------|------|------|-------|
| level | int | 等级 | 10 |
| stat_value | int | 属性值 | 2000 |
| growth_rate | float | 增长率 | 0.1 |
| marginal_utility | float | 边际效用 | 0.9 |
| is_decreasing | boolean | 是否递减 | true |

---

### 3. 概率系统配置表

#### 3.1 掉落概率表
| 字段名 | 类型 | 说明 | 示例值 |
|-------|------|------|-------|
| item_id | int | 物品ID | 3001 |
| item_name | string | 物品名称 | "普通武器" |
| rarity | string | 稀有度 | "common"/"rare"/"epic"/"legendary" |
| drop_rate | float | 掉落概率 | 0.5 |
| guarantee_count | int | 保底次数 | 0 |
| expected_pulls | float | 期望抽数 | 2 |

#### 3.2 抽卡配置表
| 字段名 | 类型 | 说明 | 示例值 |
|-------|------|------|-------|
| pool_id | int | 卡池ID | 4001 |
| pool_name | string | 卡池名称 | "限定卡池" |
| cost_per_pull | int | 单抽消耗 | 280 |
| ten_pull_discount | float | 十连折扣 | 1.0 |
| guarantee_type | string | 保底类型 | "hard"/"soft"/"probability_up" |
| guarantee_count | int | 保底次数 | 90 |
| guarantee_item | string | 保底物品 | "legendary" |

#### 3.3 概率验证表
| 字段名 | 类型 | 说明 | 示例值 |
|-------|------|------|-------|
| simulation_id | int | 模拟ID | 1 |
| total_pulls | int | 总抽数 | 10000 |
| legendary_count | int | 传说物品数量 | 110 |
| actual_rate | float | 实际概率 | 0.011 |
| target_rate | float | 目标概率 | 0.01 |
| deviation | float | 偏差 | 0.1 |
| is_acceptable | boolean | 是否可接受 | true |

---

## Excel公式示例

### 经济平衡计算
```
日产出总量 = SUMIF(货币产出表, "currency_type=gold", "amount")
日消耗总量 = SUMIF(货币消耗表, "currency_type=gold", "amount")
产出/消耗比 = 日产出总量 / 日消耗总量
```

### 成长曲线计算
```
本级所需经验 = base_exp * level^exponent
累计经验 = SUM(第1级到当前级的本级所需经验)
属性值 = base_stat * (1 + growth_rate)^level
边际效用 = (当前级属性值 - 上一级属性值) / 上一级属性值
```

### 概率期望计算
```
单抽期望 = 1 / drop_rate
保底期望 = guarantee_count * (1 - drop_rate)^guarantee_count + ...
期望付费 = 期望抽数 * cost_per_pull
```

---

## 数据可视化

### 建议图表
1. **经济平衡图**：折线图，展示每日产出/消耗/累计余额
2. **成长曲线图**：折线图，展示等级-属性成长曲线
3. **概率分布图**：柱状图，展示不同稀有度的概率分布
4. **边际效用图**：折线图，展示等级-边际效用递减曲线

### 图表工具
- Excel内置图表
- Google Sheets图表
- Python matplotlib/seaborn（批量生成）

---

## 使用说明

1. **创建Excel文件**：命名为`[系统名称]_数值表格.xlsx`
2. **按类型创建Sheet**：如"经济系统-产出表""经济系统-消耗表"等
3. **填充配置数据**：按表格结构填充具体数值
4. **添加计算公式**：使用Excel公式自动计算衍生指标
5. **生成可视化图表**：用图表验证数值合理性
6. **导出配置**：将配置表导出为JSON/CSV供游戏使用

## AI辅助要点

- AI可以生成Excel公式和计算逻辑
- AI可以设计验证方法和模拟场景
- AI可以提供参数取值范围建议
- 人类需要实际创建Excel文件并填充数据
- 人类基于经验调整具体数值
- 人类验证图表可视化结果
