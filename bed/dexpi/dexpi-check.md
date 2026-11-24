# DEXPI 校验报告

工具：pyDEXPI 2.1.0（DEXPI 2.0.1 数据模型）+ 内置校验规则。

| 图纸 | 设备 | 管段 | 仪表回路 | 问题 | 其中 MANDATORY |
|---|---:|---:|---:|---:|---:|
| pid/P-101.xml | 0 | 0 | 0 | 0 | 0 |
| pid/P-102.xml | 0 | 0 | 0 | 0 | 0 |
| pid/P-103.xml | 0 | 0 | 0 | 0 | 0 |
| pid/P-104.xml | 0 | 0 | 0 | 0 | 0 |
| pid/P-105.xml | 0 | 0 | 0 | 0 | 0 |
| pid/P-106.xml | 0 | 0 | 0 | 0 | 0 |
| pid/P-107.xml | 0 | 0 | 0 | 0 | 0 |
| pid/P-108.xml | 0 | 0 | 0 | 0 | 0 |
| pid/P-109.xml | 0 | 0 | 0 | 0 | 0 |
| pid/P-110.xml | 0 | 0 | 0 | 0 | 0 |
| pid/P-111.xml | 0 | 0 | 0 | 0 | 0 |
| pid/P-112.xml | 0 | 0 | 0 | 0 | 0 |
| pid/P-113.xml | 0 | 0 | 0 | 0 | 0 |
| pid/P-114.xml | 0 | 0 | 0 | 0 | 0 |
| process/BFD.xml | 0 | 0 | 0 | 2 | 0 |
| process/PFD.xml | 0 | 0 | 0 | 4 | 0 |

合计：16 张图纸，6 条规则问题，0 个加载错误。

## 规则问题（前 50 条）

- `process/BFD.xml` [RECOMMENDED] pydexpi_composition_fractions：Composition (S75_composition): moleFractions do not add up to their unit
- `process/BFD.xml` [RECOMMENDED] pydexpi_composition_fractions：Composition (S20_composition): moleFractions do not add up to their unit
- `process/PFD.xml` [RECOMMENDED] pydexpi_composition_fractions：Composition (S75_composition): moleFractions do not add up to their unit
- `process/PFD.xml` [RECOMMENDED] pydexpi_composition_fractions：Composition (S20_composition): moleFractions do not add up to their unit
- `process/PFD.xml` [RECOMMENDED] pydexpi_composition_fractions：Composition (S16_composition): moleFractions do not add up to their unit
- `process/PFD.xml` [RECOMMENDED] pydexpi_composition_fractions：Composition (S19_composition): moleFractions do not add up to their unit