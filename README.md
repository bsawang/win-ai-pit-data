# Windows 避坑指南 · 数据仓库

只存坑数据，不含代码。代码仓库见 [win-ai-pit](https://github.com/bsawang/win-ai-pit)。

## 结构

- `pitfalls/` — 坑数据（Markdown + YAML frontmatter），每条坑一个文件
- `kb.yaml` — 知识库 schema 定义（`windows_pitfall` 类型字段）

## 使用

由 AI 通过 MCP 工具自动操作，无需手动处理：

- **查坑**：`search_pitfall`（本地 SQLite 索引，安装后自动建立）
- **记坑**：`record_pitfall` → 写 `pitfalls/*.md` → `add/` 分支 → PR → GitHub Actions auto-merge → master

## 安装

```bash
pip install git+https://github.com/bsawang/win-ai-pit.git   # 代码
git clone https://github.com/bsawang/win-ai-pit-data.git ~/.windows-pitfalls  # 数据
windows-pitfalls init                                      # 建索引
```
