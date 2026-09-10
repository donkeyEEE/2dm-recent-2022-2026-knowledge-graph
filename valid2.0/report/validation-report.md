# valid2.0 最终图谱验证报告

状态：valid2.0 已完成离线结构、来源定位、人工审核结果绑定与页面验证；2,043 条均由 `LYZ` 完成人工审核。本报告只覆盖 `final-graph`，不报告 extraction-stage 结果。

## 1. 发布合同

- 数据版本：`valid2.0`；接口版本：`3.0.0`。
- dataset hash：`4507907510ded7f88af950b98872aaf6f0d74b6f6a4db0896968d0b27b946e1f`。
- sample manifest SHA-256：`b50922b03b8b6ae641cb29cb11d666e53a271ab991d5bc1594f82a4bec9eacdc`。
- 恢复原 1.0.0 manifest 的 250 篇冻结顺序与 B01–B05 分配，每批 50 篇；没有重新抽样。
- 仅发布 `final-graph`。全部纳入 `exhibits`、`is_variant_of`、`explained_by`，不做 `floor(n/2)` 抽边。
- `reports` 与 `uses_mechanism` 均排除。用户写法 `use_machanism` 已按源图实际谓词 `uses_mechanism` 解析。
- valid1.0 的完整 active surface 已保存到 `valid/archive/dataset-valid1.0/`，旧 1.0.0 描述符、清单与审核快照仍在 `valid/archive/dataset-1.0.0/`。

## 2. 数据规模

| 指标 | 数量 |
| --- | ---: |
| 冻结文章 | 250 |
| 有纳入关系的文章 | 247 |
| 零纳入关系文章 | 3 |
| 审核记录 | 2,043 |
| 唯一 article–SPO | 2,033 |
| 唯一 SPO | 2,014 |
| 唯一原始 edge locator | 2,029 |
| 来源待核查 | 0 |
| 带异常标记记录 | 0 |
| 缺失端点记录 | 0 |

| 谓词 | 记录数 | 涉及文章 |
| --- | ---: | ---: |
| `exhibits` | 1,124 | 238 |
| `explained_by` | 726 | 242 |
| `is_variant_of` | 193 | 92 |

| 批次 | 文章 | 记录 |
| --- | ---: | ---: |
| B01 | 50 | 392 |
| B02 | 50 | 404 |
| B03 | 50 | 403 |
| B04 | 50 | 432 |
| B05 | 50 | 412 |

## 3. 审核状态

2,043 条纳入记录均由 `LYZ` 完成人工审核：支持 1,277、部分支持 475、不支持 291。其中，原先因题名和摘要证据不足而单列的 37 条已并入不支持，其分类保留在 `error_types` 字段中；公开结果的 `reason`、`suggestion` 与 `reviewed_at` 内容统一留空。valid2.0 只保留 `human_review`，不设置 Agent 预审栏目。人工判断是摘要层面的关系审核，不构成独立科学真实性证明。

canonical 快照没有漏抽候选；如后续新增，仍须置于既有关系分母之外，且不能据此声称严格召回率。

## 4. 可追溯性与验证

- 原 1.0.0 数据通过归档 manifest 和只读源文件确定性重建；final-graph articles、records、nodes、source-pending 与 shared articles 的 SHA-256 均与原 descriptor 完全一致，然后才应用 valid2.0 谓词过滤。
- 独立 verifier 逐条解析 `graph_data/knowledge_graph.json` 的 JSON Pointer，核对 2,043 条 raw edge 与 1,997 个 endpoint node，并按完整 `source_article` 标识独立重算每篇记录数。
- canonical 人工审核结果通过严格版本/hash、文章、批次、record ID、字段、审核人 `LYZ` 与时间戳校验。
- 原始源图 SHA-256 为 `5b240ce0e4b42dd3ee587ae14be24c687c77d885e894934c52ca631e1cb136f3`，发布前后未变化。
- 未执行 live provider、Neo4j、embedding、GraphRAG 或图清理操作。

详细机器证据见 `shared/dataset.json`、`shared/release-migration.json`、`final-graph/stage-statistics.json` 与 `report/verification.json`。

## 5. 交付物与限制

- 审核页面：`final-graph/index.html`。
- canonical 结果：`final-graph/results/final-graph__human-reviewed__valid2.0.json`。
- 页面含 250 篇与 2,043 条关系，只显示人工审核栏目，并支持结果编辑、导入和导出。
- 当前环境未安装 Playwright，但项目自带 CDP/Chromium 脚本覆盖 250 篇/五批导航、2,043 条分母、导出确认、关闭重开后导入恢复、错误绑定拒绝、重复记录隔离、零关系与缺失端点夹具、桌面截图及移动端无横向溢出；本次协议迁移后重新执行浏览器验证。
