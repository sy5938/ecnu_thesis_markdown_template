# Repository Guidelines

## Project Structure & Module Organization
本仓库用于用 Quarto 写作并输出 ECNU 论文 PDF，包含硕士版和博士版两个独立子项目：
- `master/`：硕士学位论文模板
- `doctoral/`：博士学位论文模板

每个版本内部结构相同：
- `_quarto.yml`：项目入口配置与章节清单。
- `index.qmd`：中英文摘要。
- `chapters/`：正文各章，每章一个文件（如 `chapter01.qmd`）。
- `tex/`：LaTeX 注入与封面/声明页（`tex/preamble.tex`、`tex/before-body.tex`、`tex/ecnu/*.tex`）。
- `fig/`：图片与其他资源。
- 输出目录 `_book/` 为自动生成，不要提交。

## Build, Test, and Development Commands
本项目只需渲染，不含自动化测试：
```bash
cd master   # 或 cd doctoral
quarto render
```
渲染整本书并生成 PDF。
```bash
quarto render chapters/chapter03.qmd
```
仅渲染单章，便于快速预览。

## Coding Style & Naming Conventions
- QMD：标题用 `#`/`##` 等 Markdown 规范，章节文件命名为 `chapter01.qmd`、`chapter02.qmd` 等。
- YAML：`_quarto.yml` 使用 2 空格缩进。
- 资源命名保持语义清晰，图片放在 `fig/`。

## Testing Guidelines
暂无单元测试或测试框架。修改排版或章节结构后，请手动执行 `quarto render` 并检查 PDF 版式与目录条目。

## Commit & Pull Request Guidelines
当前历史提交使用简短、祈使语气的英文描述（如 "update README.md."）。建议保持：
- 主题简洁、动作导向（例如 "Update chapter titles"）。
- 若改动影响排版或封面，请在 PR 中附上渲染后的 PDF 或截图。
- 若新增章节，记得同步更新 `_quarto.yml` 的 `book.chapters` 列表。

## Environment Notes
需要 TeX Live（含 `ctex`）与 XeLaTeX。若出现缺包，可使用 `tlmgr install <package>` 补齐（例如 `tlmgr install ctex`）。
