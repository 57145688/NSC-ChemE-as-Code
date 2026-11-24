# DWSIM 工艺仿真（BED）

本目录是天然气制氨工艺的 DWSIM 仿真输入与结果归档。

| 文件 | 含义 |
|---|---|
| `simulation-spec.yaml` | cheme 的 DWSIM 模型定义：11 个组分、Peng-Robinson、撕裂流、设备规格、排除项 |
| `simulation-run-archived.json` | 已验证的 DWSIM 9.0.5 收敛记录：`converged: true`，137 次迭代，逐设备状态 |
| `stream-table.csv` | `cheme process streams --csv` 的全厂流股表（含来源标记） |

## 重新运行

需要 DWSIM 9.0.5（Windows 安装版，含 .NET）。

```powershell
# 在 ammonia-bed 根目录
cheme process simulate --check                 # 只校核，不写回
cheme process simulate --fill                  # 把计算值写回 flowsheet 并记录 simulation-run.json
cheme process simulate --save bed/dwsim/ammonia-bed.dwxmz   # 另存 DWSIM 工作簿
```

`--save` 需要 DWSIM 的 .NET 自动化接口可用；若未安装，`cheme process simulate` 会以
“no simulator” 退出。此时仍可使用已归档的 `simulation-run-archived.json` 作为
本 BED 包的仿真证据。

## 边界与已知限制

- 未模拟：H-101 辐射段、U-801 蒸汽系统、K-701 冷冻回路（见 spec 的 `exclude`）。
- aMDEA  CO2 脱除按指定分割处理，不是严格的电解质/吸收模型。
- 物性：Peng-Robinson；缺少有来源的 PR kij，合成回路 NH3 冷凝与溶解气体是薄弱点。
- 所有结果为概念/BED 方案级，不构成工艺保证。
