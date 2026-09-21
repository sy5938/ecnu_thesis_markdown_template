# ECNU 学位论文 Markdown 模板

本仓库用于在 Quarto 中直接写作并编译华东师范大学学位论文 PDF（XeLaTeX + ctexbook），复用 ECNU LaTeX 模板的封面/声明页、参考文献样式与目录/图表编号规范。

仓库提供 **硕士版**（`master/`）和 **博士版**（`doctoral/`）两个完全独立的 Quarto 子项目，两者结构对称、可分别编译，互不影响。

## 目录结构

```
├── master/                  # 硕士学位论文模板（独立 Quarto 项目）
├── doctoral/                # 博士学位论文模板（独立 Quarto 项目）
├── ecnu_notification_doc/   # 学校官方格式说明文档（参考，非模板代码）
├── ECNU_thesis_xxx.pdf      # 早期渲染示例（历史文件，非当前结构产物）
└── README.md
```

`master/` 与 `doctoral/` 内部结构相同：

- `_quarto.yml`：Quarto 项目配置（书籍类型、PDF 输出、ctexbook 文档类）
- `index.qmd`：中英文摘要 + 目录/图目录/表目录的排版设置
- `chapters/`：正文各章，一章一个文件（`chapter01.qmd`、`chapter02.qmd` …）
- `references.qmd` + `references.bib`：参考文献
- `tex/preamble.tex`：页眉、字体、目录/图表目录样式、图表公式编号规则等全局设置
- `tex/before-body.tex`：`\begin{document}` 之后立即插入的内容——封面、声明页、章节标题字号
- `tex/after-body.tex`：正文结束、`\end{document}` 之前插入的内容——后置页
- `tex/ecnu/`：封面、原创性声明、AI 工具使用声明、导师签字页等独立页面
- `fig/`：校徽等图片资源
- `GBT7714-2005*.bst`：国标顺序编码制参考文献样式

> `include-before-body` / `include-after-body` 是 Quarto/Pandoc 的标准机制，分别对应 `\begin{document}` 之后、`\end{document}` 之前两个固定插入点，与 `index.qmd` 在 `book.chapters` 列表中的位置无关；调整 `before-body.tex`/`after-body.tex` 内 `\input` 的先后顺序即可控制前置页/后置页的排列顺序。

### 前置页顺序（硕士版、博士版相同）

1. 中文封面 → 2. 英文封面 → 3. 原创性声明 + 著作权声明 → 4. AI 工具使用声明 → 5. 导师和指导小组同意答辩确认声明 → 6. 答辩委员名单

### 后置页（硕士版与博士版不同，按各自要求配置）

- **硕士版**：发表论文和科研情况（`tex/achievements.tex`）→ 致谢（`tex/acknowledgement.tex`）
- **博士版**：作者简历及在学期间科研成果（`tex/author-resume.tex`）→ 致谢（`tex/acknowledgement.tex`）→ 附：博士其他材料——答辩决议、导师评阅表、开题/预答辩/答辩签名单占位页（`tex/ecnu/A7-SUPPLEMENTARY.tex`）

## 使用方法

1. 进入对应版本目录：`cd master` 或 `cd doctoral`
2. 修改论文标题、院系等基本信息：`tex/preamble.tex` 末尾的 `\thesisTitle` 等宏，以及 `tex/ecnu/*.tex` 里的占位内容
3. 撰写摘要：`index.qmd`
4. 撰写正文：`chapters/chapter01.qmd` 等，新增章节需同步在 `_quarto.yml` 的 `book.chapters` 列表中登记
5. 插入公式：使用 Quarto 数学语法并加 label 即可自动按章节编号（如第三章第一个公式自动生成 `(3-1)`）：

   ```markdown
   $$
   y = \beta_0 + \beta_1 x + \epsilon
   $$ {#eq-example}
   ```

6. 渲染 PDF：

   ```bash
   quarto render                        # 渲染整本书
   quarto render chapters/chapter03.qmd # 仅渲染单章，便于快速预览
   ```

渲染结果位于各自目录下的 `_book/`（已在 `.gitignore` 中忽略，不要提交）。

## 环境要求

- [Quarto](https://quarto.org/)
- TeX Live（含 `ctex` 宏包）与 XeLaTeX，缺包时可用 `tlmgr install <package>` 补齐

## 当前版本改动说明

在硕士/博士双版本拆分的基础上，本版本重点修复了目录、图表目录与 PDF 书签的排版细节：

- **目录（TOC）层级区分**：一级条目（章）黑体四号、二级及以下（节/小节）黑体小四，一级条目前增加段前间距，使章节层级在视觉上更清晰；字号统一改用 ctex 原生的 `\zihao` 命令而非手写 `\fontsize`，避免中文字号不随设置变化的问题
- **黑体粗细修复**：macOS 下 ctex 的 `\heiti` 默认映射到偏细的系统字体「Heiti SC Light」，明显比 Windows 黑体（SimHei）纤细；已改为「Heiti SC Medium」，目录、正文章节标题等所有黑体文本均受益
- **目录点状引导线**：由稀疏的点线（点间距 9.5pt）改为紧密点线（4pt），贴近学校模板视觉效果
- **图目录/表目录**：去除不同章节之间的多余间距；修复超链接跳转位置错误的问题
- **PDF 书签（大纲）**：
  - 目录标题补充书签锚点，"目录"会出现在 PDF 侧边栏中
  - 开启 `bookmarksnumbered`，书签自动带上"第一章""1.1""1.2.1"等章节编号（前提是该级标题未标记 `{.unnumbered}`）
- **公式编号**：新增按章节自动编号支持（如 `(3-1)`），计数器随 `\chapter` 自动重置，无需手动维护
- **摘要标题**：英文摘要标题由 `Abstract` 改为大写 `ABSTRACT`
- **`.gitignore`**：忽略各子项目渲染生成的顶层 `.tex`（`keep-tex: true` 产物）与 `.DS_Store`，不影响 `tex/` 目录下的模板源文件
