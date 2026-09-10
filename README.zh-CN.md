# 二维磁性材料知识图谱（2022—2026）

[English](README.md)

本仓库保存从 2022—2026 年二维磁性材料文献中构建的独立知识图谱，以及对应的 valid2.0 人工审核发布包。

## 知识图谱

- 文件：`knowledge_graph.json`
- 节点：88,987
- 边：235,718
- 大小：109,995,800 bytes
- SHA-256：`46ea8cf5e9c6b2f25168caea4f00723fe8908c4b5da4b39682d6e4ccfd1f707c`

图谱文件使用 Git Large File Storage（Git LFS）存储。克隆或拉取仓库前，请先安装并启用 Git LFS。

## 抽取流程

1. 读取 2022—2026 年 Web of Science 工作簿，规范化论文标识，并根据 DOI 处理重复记录。
2. 以每篇论文的标题和摘要为来源文本，进行主题筛选，并结构化抽取材料实体、性质、机理、应用及其关系。
3. 校验抽取结构、关系端点、文献来源和证据支持，剔除未通过校验的关系。
4. 保留论文来源信息，将通过校验的论文级结果合并到独立图谱，并导出 `knowledge_graph.json`。

`valid2.0` 是抽取完成后对最终图谱关系样本进行的人工审核，不是抽取流程的一个环节，也不表示全图关系均已审核。

## valid2.0 人工审核发布

[`valid2.0/`](valid2.0/) 是最终图谱的自包含审核发布包，涵盖 247 篇来源文献中的 2,043 条关系，全部由 `LYZ` 完成人工审核：

- 支持：1,277
- 部分支持：475
- 不支持：291
- 通过率：85.76%（支持与部分支持合计）

“支持”表示文献标题或摘要支持所审关系，不等于独立证明科学真实性。详细范围、来源边界和限制见 [`valid2.0/README.md`](valid2.0/README.md) 与 [`valid2.0/report/validation-report.md`](valid2.0/report/validation-report.md)。

## 离线审核页面

用 Chromium、Chrome 或 Edge 直接打开：

```text
valid2.0/final-graph/index.html
```

页面已内嵌冻结数据与 canonical 人工审核结果，不需要 Web 服务或网络连接，并支持审核结果 JSON 的导入和导出。

## 完整性校验

在仓库根目录执行：

```bash
sha256sum --check valid2.0/SHA256SUMS
```

该校验清单覆盖 `valid2.0/` 中除 `SHA256SUMS` 自身之外的全部文件。
