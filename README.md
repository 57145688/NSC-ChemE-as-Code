# 天然气制氨 BED 包（ChemE-as-Code 全开源流水线）

本仓库以 [ChemE-as-Code](https://github.com/ChemE-as-Code) 的 `ammonia-1`
（1500 t/d 天然气蒸汽转化 + Haber-Bosch）为设计基线，输出一套**方案级（BED）**
可交付包，并把每个交付物接到一条可复现的开源工具链上。

> **定位：BED 方案级可用。** 用于路线比选、规模论证、下一阶段设计输入与审查准备；
> 不用于施工、采购、许可、HAZOP 定论或安全决策。所有数字均为概念级估算。

---

## 1. 上游仓库与安装

工作区结构（`E:\trae\trae workspace\nsc-ChemE-as-Code`）：

| 目录 | 来源 | 说明 |
|---|---|---|
| `core/` | `ChemE-as-Code/core` | `cheme` 工具链（validate/fmt/render/diff/process/safety/assess） |
| `process-library/` | `ChemE-as-Code/process-library` | 稀疏克隆（仅 ammonia-1、natural-gas-processing-1 及公共目录） |
| `project-template/` | `ChemE-as-Code/project-template` | 空白工厂模板 |
| `org-github/` | `ChemE-as-Code/.github` | 组织主页草稿 |
| `ammonia-bed/` | **本仓库** | BED 包 + 流水线 |
| `.venv/` | 本地 | cheme + pyDEXPI + Brightway |
| `.venv-psi/` | 本地 | PSI 隔离环境（numpy 1.x） |

安装：

```powershell
uv venv --python 3.12 .venv
uv pip install --python .venv\Scripts\python.exe -e ".\core[serve,png]"
uv pip install --python .venv\Scripts\python.exe bw2data bw2calc
```

FreeCAD 1.0 已安装于 `D:\Program Files\FreeCAD 1.0`；DWSIM 9.0.5 可从
`D:\...\DWSIM_setup.exe` 安装（见 §4）。

---

## 2. 案例验证（ammonia-1）

在原仓库案例上实际执行：

| 检查 | 命令 | 结果 |
|---|---|---|
| 工程文件校验 | `cheme validate` | **0 error / 207 warning**，exit 0；16 张图（14 张 P&ID + BFD + PFD）与 26 份清单交叉校验 |
| DEXPI 结构校验 | `python tools/dexpi_check.py` | 16 张图，pyDEXPI 6 条规则提示，**0 条 MANDATORY**，0 加载失败 |
| 三维出图 | FreeCAD 1.0 headless | 52 台设备、250 个管线对象、1642 段，`ammonia-bed.step` 3.9 MB |
| 应力初算 | `python tools/psi_batch.py` | 12 条关键管线：**5 条 PSI 出结果**（RATIO 0.02–0.11）；7 条超 PSI 管表/碳钢温度上限，给出 B31.1 保守上限 |
| LCA | `python tools/brightway_lca.py` | **1.89 kg CO2e / kg NH3**（含直接排放 + 天然气上游 + 外购电；水为 gap） |
| HAZOP | `cheme safety hazop report H-01` | 16 节点、72 个偏差场景、风险矩阵与建议措施，状态 draft |

`cheme validate` 的 207 条 warning 主要来自评审记录编号、HAZOP 未决提案、
生成器声明等治理项，**不是 P&ID 结构错误**；详见运行输出与 `cheme status`。

---

## 3. 各功能怎么操作

在 `ammonia-bed/` 根目录执行（`cheme` 在 `.venv\Scripts`）：

```powershell
cheme validate                 # 校验 P&ID、清单及二者一致性
cheme fmt                      # 规范化 DEXPI XML（可读 id、稳定顺序）
cheme render pid/P-101.xml -o bed/pid/P-101.svg   # 出图（SVG；--png 需 png extra）
cheme diff main --list         # 与 git 版本比较，出变更清单/HTML
cheme show V-101               # 查对象所在图、文件、引用
cheme process build            # 由 flowsheet 生成 BFD/PFD
cheme process streams --csv    # 流股表（含 provenance）
cheme process simulate --check # 运行 process/simulation.yaml 指定的 DWSIM 模型
cheme safety hazop check       # HAZOP 完整性
cheme safety hazop report H-01 # 输出 build/hazop/H-01.md/.html
cheme assess lca --write       # 开源因子 LCA（DESNZ 2026）
cheme assess lca --coverage    # 数据质量与缺口
cheme layout build             # 由清单/P&ID 生成三维布局
cheme status                   # 全厂状态、门禁、签核
```

一键流水线：

```powershell
python tools/run_pipeline.py
# 或
python tools/run_pipeline.py --skip-cad
```

流水线依次执行：`cheme validate` → pyDEXPI 校验 → 流股表 → cheme LCA →
Brightway LCA → PSI 批量应力 → HAZOP 工作表/报告 → FreeCAD STEP → `bed/MANIFEST.md`。

---

## 4. 全开源工具链映射

| 环节 | 采用工具 | 本包落地方式 | 状态/边界 |
|---|---|---|---|
| 工艺仿真 | **DWSIM 9.0.5** | `process/simulation.yaml` + 收敛记录 `simulation-run.json`；`bed/dwsim/simulation-summary.md` 汇总 62 个单元（59 simulated / 3 excluded，137 次迭代收敛） | 本机 DWSIM 安装包 272 MB，当前网络约 11 KB/s，未完成安装；归档记录来自 DWSIM 9.0.5 实际运行。重跑：`cheme process simulate --fill` 或 `CHEME_DWSIM_PATH=<安装路径>` |
| P&ID 绘图 | **G-PID**（Godot-PID，MIT） | `pid/*.xml` 为 DEXPI 2.0.1；G-PID v0.1 支持 DEXPI 1.4 导入/导出 | **版本差**：G-PID 需 1.4，cheme 用 2.0.1；跨版本需经 pyDEXPI/Proteus 转换后编辑 |
| DEXPI 读写/校验 | **pyDEXPI 2.1.0** | `tools/dexpi_check.py`：加载 + 规则校验 + 对象统计 | 直接读写 DEXPI 2.0.1 |
| 三维设备/管线 | **FreeCAD 1.0 + Part** | `tools/freecad_build.py`：设备实体 + 管线扫掠 + STEP | **ProcessWB / PipeFeatures 未检索到可用公开发行版**，用 FreeCAD 原生 Part 实现等价分组（Equipment/Piping/Structures）；对象名保留 `PF_<line>_<segment>` 以便接入 |
| 管线建模备选 | **PipeCAD** | 未纳入本次运行 | 作为后续替换/对照 |
| 管道应力 | **PSI（Pipe Stress Infinity）** | `tools/psi_batch.py` 生成 `.inp` 并批量运行；5 条管线得到 B31.1 CODE 应力与 RATIO | PSI 0.0.1 内置管表只覆盖到 NPS 48，碳钢材料温度上限约 500 ℃；超范围管线改用完全约束热伸长保守上限，并标注为筛查值 |
| LCA | **Brightway 2.5** | `tools/brightway_lca.py`：biosphere + technosphere + IPCC 2021 GWP100 | 背景仅用公开 DESNZ 2026 因子；去离子水为 gap；非验证 LCA |
| HAZOP | **pyHazop 兼容工作表** | `tools/hazop_export.py` 从 cheme HAZOP 模型导出 CSV/Markdown | 公开 `pyhazop` 仓库仅为网页演示，无可调用分析包；本包以 cheme HAZOP 数据模型为源，按 pyHazop 列导出 |
| CI | GitHub Actions | `.github/workflows/bed-ci.yml` | Windows runner；cheme validate / DEXPI / PSI / FreeCAD STEP / 上传 BED 包 |

---

## 5. BED 交付物（`bed/`）

| 路径 | 内容 |
|---|---|
| `process/design-basis.yaml` | 设计基准：1500 t/d、8000 h/a、天然气/空气/脱盐水、产品与公用工程 |
| `pid/*.xml` + `bed/pid/*.svg` | DEXPI P&ID（14 张）与渲染图 |
| `bed/dwsim/` | DWSIM 模型定义、收敛记录、全厂流股表 |
| `bed/cad/ammonia-bed.step` | FreeCAD 三维设备+管线 STEP（3.9 MB） |
| `bed/psi/stress-report.md` | 12 条关键管线应力初算与保守上限 |
| `bed/lca/brightway-result.md` | 1.89 kg CO2e/kg NH3 筛选碳足迹与缺口 |
| `bed/hazop/hazop-draft.md` + `hazop-worksheet.csv` | HAZOP 初稿（72 场景）与 pyHazop 兼容工作表 |
| `bed/dexpi/dexpi-check.md` | pyDEXPI 校验报告 |
| `bed/MANIFEST.md` / `.json` | 交付物清单、大小、SHA256 前 16 位 |

---

## 6. 许可与归属

- `core`、`process-library` 的脚本/工具：**AGPL-3.0-or-later**。
- `process-library` 的设计数据：**CC BY 4.0**（见其 `LICENSE-DATA`）；本包保留来源与修改说明。
- G-PID：**MIT**；FreeCAD：**LGPL-2.1+**；pyDEXPI：**Apache-2.0**；PSI：**GPL-3.0**；
  Brightway：**BSD-3-Clause**；DESNZ 因子：**OGL-3.0**。
- 本包中的氨厂设计继承自 ChemE-as-Code Process Atlas 的 `ammonia-1`，
  属 AI 辅助生成、未经独立人工评审的研究输出。
