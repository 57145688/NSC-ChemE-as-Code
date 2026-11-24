# 管道应力初算报告（BED 方案级）

PSI 可执行文件：`E:\trae\trae workspace\nsc-ChemE-as-Code\.venv-psi\Scripts\psi.exe`；关键管线 12 条（按 DN、压力、温度排序）。

**工具状态**：PSI 0.0.1 对 NPS ≤ 48、设计温度 ≤ 500 ℃ 的管线可完成 B31.1 计算；超出 PSI 内置管表或碳钢材料温度范围的大口径高温管线，标为 `psi-section-unavailable` / `psi-temperature-out-of-range`，改用保守上限。两类结果都只用于筛查。

| 管线 | DN | 流体 | 长度 m | 设计压力 bar | 设计温度 °C | PSI CODE MPa | PSI RATIO | 保守热上限 MPa | 结论 | PSI 状态 |
|---|---:|---|---:|---:|---:|---:|---:|---:|---|---|
| FL-1014 | 4000 | Flue gas | 30.0 | 5.0 | 1075.0 |  |  | 2532 | 需详细柔性分析 | psi-section-unavailable |
| FL-1015 | 3600 | Flue gas | 30.0 | 5.0 | 789.4 |  |  | 1847 | 需详细柔性分析 | psi-section-unavailable |
| FL-1016 | 3400 | Flue gas | 30.0 | 5.0 | 633.5 |  |  | 1472 | 需详细柔性分析 | psi-section-unavailable |
| FL-1017 | 3000 | Flue gas | 30.0 | 5.0 | 519.5 |  |  | 1199 | 需详细柔性分析 | psi-section-unavailable |
| PG-1032 | 900 | Process gas | 12.77 | 37.4 | 1024.7 |  |  | 2411 | 需详细柔性分析 | psi-temperature-out-of-range |
| PG-1098 | 450 | Synthesis gas | 30.28 | 166.1 | 455.0 | 8.51 | 0.11 | 1044 | PSI: 初算通过 | psi-ok |
| PG-1009 | 700 | Process gas | 64.21 | 38.5 | 835.0 |  |  | 1956 | 需详细柔性分析 | psi-temperature-out-of-range |
| PG-1099 | 400 | Synthesis gas | 18.13 | 165.0 | 284.5 | 8.34 | 0.1 | 635 | PSI: 初算通过 | psi-ok |
| PG-1096 | 400 | Synthesis gas | 31.17 | 169.4 | 245.7 | 8.71 | 0.11 | 542 | PSI: 初算通过 | psi-ok |
| PG-1008 | 500 | Process gas | 22.33 | 45.1 | 645.0 |  |  | 1500 | 需详细柔性分析 | psi-temperature-out-of-range |
| PG-1034 | 700 | Process gas | 25.5 | 36.3 | 469.0 | 4.57 | 0.06 | 1078 | PSI: 初算通过 | psi-ok |
| CO2-1066 | 1200 | CO2 and water vapour | 53.08 | 5.5 | 75.0 | 1.4 | 0.02 | 132 | PSI: 初算通过 | psi-ok |

## 使用边界

- 本报告只用于 BED 方案级：识别高温、大口径、高压管线是否需要详细柔性分析。
- 完全约束热伸长是上限；真实管系通过弯头、膨胀节、支吊架释放，PSI/CAESAR 详细分析后才能判定。
- 未计入风、地震、水锤、两相流、冷紧、支架摩擦与法兰泄漏。
- 设计压力按 1.1 × 操作压力、设计温度按操作温度 +25 K 假设，需由工艺专业确认。