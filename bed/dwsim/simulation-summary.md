# DWSIM 仿真结果摘要

- 模拟器：dwsim 9.0.5.0
- 物性方法：Peng-Robinson (PR)
- 收敛：True（converged，137 次迭代）
- 运行时间：2026-10-30T08:27:57Z；记录工具：cheme-as-code 1.0.1
- 设备/单元：62 个（simulated 59，excluded 3）

## 单元状态

| 单元 | 类型 | 状态 | 收敛 | 依据 |
|---|---|---|---|---|
| SPL101 | splitter | simulated | True | specified |
| E101 | heat_exchanger | simulated | True | predictive |
| D101 | heater | simulated | True | predictive |
| M101 | mixer | simulated | True | predictive |
| E102 | heat_exchanger | simulated | True | predictive |
| R100 | conversion_reactor | simulated | True | specified |
| E103 | heat_exchanger | simulated | True | predictive |
| R101 | equilibrium_reactor | simulated | True | predictive |
| K101 | compressor | simulated | True | predictive |
| K101D | compressor | simulated | True | predictive |
| E104 | heat_exchanger | simulated | True | predictive |
| M103 | mixer | simulated | True | predictive |
| R102A | conversion_reactor | simulated | True | specified |
| R102B | equilibrium_reactor | simulated | True | predictive |
| E105 | cooler | simulated | True | predictive |
| K102 | compressor | simulated | True | predictive |
| E106 | heat_exchanger | simulated | True | predictive |
| M102 | mixer | simulated | True | predictive |
| E107 | cooler | simulated | True | predictive |
| K103 | compressor | simulated | True | predictive |
| R201 | equilibrium_reactor | simulated | True | predictive |
| E201 | cooler | simulated | True | predictive |
| R202 | equilibrium_reactor | simulated | True | predictive |
| E202 | cooler | simulated | True | predictive |
| E203 | cooler | simulated | True | predictive |
| V201 | flash | simulated | True | predictive |
| P301 | pump | simulated | True | predictive |
| M301 | mixer | simulated | True | predictive |
| C301 | component_separator | simulated | True | specified |
| E401 | heat_exchanger | simulated | True | predictive |
| R401 | conversion_reactor | simulated | True | specified |
| E402 | cooler | simulated | True | predictive |
| V401 | flash | simulated | True | predictive |
| X401 | component_separator | simulated | True | specified |
| M501 | mixer | simulated | True | predictive |
| K501 | compressor | simulated | True | predictive |
| M502 | mixer | simulated | True | predictive |
| K502 | compressor | simulated | True | predictive |
| E501 | heat_exchanger | simulated | True | predictive |
| R501 | equilibrium_reactor | simulated | True | predictive |
| E502 | cooler | simulated | True | predictive |
| E503 | cooler | simulated | True | predictive |
| E505 | heat_exchanger | simulated | True | predictive |
| E504 | cooler | simulated | True | predictive |
| V501 | flash | simulated | True | predictive |
| SPL501 | splitter | simulated | True | specified |
| LV501 | valve | simulated | True | predictive |
| V502 | flash | simulated | True | predictive |
| V701 | flash | simulated | True | predictive |
| K702 | compressor | simulated | True | predictive |
| V702 | flash | simulated | True | predictive |
| E702 | cooler | simulated | True | predictive |
| V703 | flash | simulated | True | predictive |
| M701 | mixer | simulated | True | predictive |
| M601 | mixer | simulated | True | predictive |
| X601 | component_separator | simulated | True | specified |
| E601 | cooler | simulated | True | predictive |
| X602 | component_separator | simulated | True | specified |
| K601 | compressor | simulated | True | predictive |
| H101 | None | excluded | None |  |
| K701 | None | excluded | None |  |
| U801 | None | excluded | None |  |

## 说明

- 本摘要来自 `process/simulation-run.json` 中 DWSIM 9.0.5 的实际收敛记录。
- 本机未安装 DWSIM 9.0.5（安装包 272 MB，当前网络约 11 KB/s，未完成下载），因此没有重新运行；重新运行的命令见 `bed/dwsim/README.md`。
- 结果只覆盖 `process/simulation.yaml` 声明的范围：排除了 H101、U801、K701；aMDEA 为指定分割；PR 缺少有来源的 kij。