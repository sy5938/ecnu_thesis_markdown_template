# sywu-thesis

本仓库用于在 Quarto 中直接写作与编译（`quarto render`），并复用 ECNU LaTeX 模板的封面/声明页与参考文献样式。

## 目录结构

- `_quarto.yml`：Quarto 配置（PDF + ctexbook）
- `index.qmd`：中英文摘要
- `chapters/`：正文各章（每章一个文件）
- `tex/`：LaTeX 注入文件
  - `tex/preamble.tex`：页眉/字体/目录样式 + 封面宏
  - `tex/before-body.tex`：插入封面/声明页与目录
  - `tex/ecnu/`：从 ECNU 模板复制的 A1-A4 页
- `fig/`、`resources/`：封面与正文图像

## 使用

1. 修改封面信息：`tex/preamble.tex` 与 `tex/ecnu/*.tex`
2. 写正文：`index.qmd` 与 `chapters/chapter01.qmd` 等
3. 渲染 PDF：

```bash
quarto render
```


