# valid2.0 人工审核发布

本目录是近期二维磁性材料图谱的自包含 `final-graph` 审核发布。它绑定 250 篇冻结文献和 2,043 条明确归属关系；所有关系均由 `LYZ` 完成人工审核。valid2.0 仅使用 `human_review`，不设置 Agent 预审栏目。

## 审核结果

- 支持：1,277
- 部分支持：475
- 不支持：291
- 通过（支持＋部分支持）：1,752（85.76%）
- 未通过：291（14.24%）

“支持”表示标题或摘要支持所审关系，不等于独立证明科学真实性。完整范围、来源边界和限制见 [验证报告](report/validation-report.md) 与 [接口](shared/INTERFACE.md)。
公开结果保留 `reason`、`suggestion` 与 `reviewed_at` 字段以维持结构兼容，但其内容统一为空字符串。

## 离线查看

用 Chromium、Chrome 或 Edge 直接打开 `final-graph/index.html`。页面已内嵌冻结数据和 canonical 人工审核结果，不需要 Web 服务或网络连接，并支持审核结果 JSON 的导入与导出。

## 校验

在仓库根目录执行：

```bash
sha256sum --check valid2.0/SHA256SUMS
```

`SHA256SUMS` 覆盖 `valid2.0/` 中除其自身外的全部文件。机器可读绑定见 `shared/dataset.json`、`final-graph/stage-statistics.json` 和 `report/delivery-manifest.json`。
