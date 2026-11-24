# P&ID 出图与 G-PID 编辑

## 本目录内容

- `*.svg`：14 张 DEXPI P&ID 的 `cheme render` 渲染图（A1/A2 图幅，矢量）。
- 源 XML 在仓库根 `pid/P-101.xml` … `P-114.xml`（DEXPI 2.0.1）。

## G-PID 工作流

[G-PID（Godot-PID）](https://github.com/jonsonw/Godot-PID) 是 MIT 许可的 Godot 4 开源
P&ID 编辑器，v0.1 支持 **DEXPI 1.4（Proteus XML 4.2.0）** 导入/导出。

由于 cheme 使用 **DEXPI 2.0.1**，两者不能直接互开。推荐流程：

1. 在 G-PID 中完成符号、连线、标题栏与 DRC 检查，导出 DEXPI 1.4 XML。
2. 用 pyDEXPI 读入 1.4 模型，按 DEXPI 2.0.1 序列化（必要时补齐 mandatory 属性）。
3. 放入本仓库 `pid/`，执行 `cheme fmt`、`cheme validate`、`cheme render`。
4. 反向修改时，从 `pid/*.xml` 导出 1.4 给 G-PID，避免直接手改 2.0.1 XML。

> G-PID v0.1 的 3D 联动仍是空桩，跨图连接/off-page 连接符尚未实现；
> 本项目因此把 G-PID 定位为“绘图前端”，一致性校验与交付版本仍以 cheme + pyDEXPI 为准。
