---
title: Pandoc构建电子书
created: 2025-09-02 09:43
tags:
  - Software
---
<!-- markdownlint-disable MD025 -->

# Pandoc构建电子书

使用 Pandoc 将多个 Markdown 文件合并并构建电子书 PDF 的完整方案，结合了文件合并、格式转换、中文支持及专业排版优化：

---

## 📚 **1. 合并多个 Markdown 文件**

- **方法一：命令行合并（推荐）**  
     使用 `cat` 命令（Linux/macOS）或 PowerShell（Windows）将所有 `.md` 文件按顺序合并：

     ```bash
     # Linux/macOS
     cat chapter1.md chapter2.md chapter3.md > combined.md

     # Windows PowerShell
     Get-Content chapter1.md, chapter2.md, chapter3.md | Out-File combined.md
     ```

- **方法二：Pandoc 直接合并**  
     跳过中间文件，Pandoc 支持多文件输入直接生成 PDF：

     ```bash
     pandoc chapter1.md chapter2.md chapter3.md -o ebook.pdf
     ```

---

## ⚙️ **2. 转换为电子书 PDF（基础命令）**

   ```bash
   pandoc combined.md -o ebook.pdf --toc --pdf-engine=xelatex
   ```

- `--toc`：自动生成目录（电子书必备）  
- `--pdf-engine=xelatex`：支持中文排版和复杂字体  

---

## 🇨🇳 **3. 解决中文显示问题**

   **步骤：**  

   1. **指定中文字体**（避免乱码或空白）：  

      ```bash
      pandoc combined.md -o ebook.pdf --pdf-engine=xelatex -V CJKmainfont="Microsoft YaHei"
      ```

      > ✅ 常用中文字体：`SimSun`（宋体）、`KaiTi`（楷体）、`Microsoft YaHei`（微软雅黑）  

   2. **自定义 LaTeX 模板（可选）**  
      若默认模板不支持中文，生成自定义模板并添加 `ctex` 包：

      ```bash
      pandoc -D latex > template.tex  # 导出默认模板
      # 在 template.tex 的文档开头添加：\usepackage{ctex}
      pandoc combined.md -o ebook.pdf --template=template.tex --pdf-engine=xelatex
      ```

---

## 📖 **4. 电子书专业优化**

- **自动章节编号**：  

     ```bash
     pandoc combined.md -o ebook.pdf -N --toc --pdf-engine=xelatex
     ```

     `-N`：为标题添加自动编号（如 1.1、1.2）  

- **调整页边距与版式**：  

     ```bash
     pandoc combined.md -o ebook.pdf -V geometry:"top=2cm, bottom=2cm, left=2cm, right=2cm"
     ```

- **添加元数据（标题、作者）**：  

     ```bash
     pandoc combined.md -o ebook.pdf --metadata title="电子书标题" --metadata author="作者名"
     ```

- **嵌入封面图片**：  

     ```bash
     pandoc combined.md -o ebook.pdf --epub-cover-image=cover.jpg
     ```

     > ⚠️ 需确保图片路径正确  

---

### 🎨 **5. 高级定制（模板与样式）**

- **使用专业模板**（如 `eisvogel`）：  
     下载模板后指定路径，优化代码块高亮和列表样式：

     ```bash
     pandoc combined.md -o ebook.pdf --template=path/to/eisvogel.latex --listings
     ```

     > 🔍 模板下载：<https://github.com/Wandmalfarbe/pandoc-latex-template>  

- **自定义 CSS（HTML 中转）**：  
     若需精细控制样式，可先转 HTML 再转 PDF：

     ```bash
     pandoc combined.md -o temp.html --css=style.css
     pandoc temp.html -o ebook.pdf --pdf-engine=wkhtmltopdf
     ```

---

### ⚠️ **常见问题解决**

- **中文换行失败**：检查模板是否包含 `\usepackage{ctex}`，或更换模板  
- **图片路径错误**：确保图片使用相对路径，或添加 `--resource-path=.:images/` 指定资源目录  
- **LaTeX 未安装**：Windows 需安装 <https://www.tug.org/texlive/，Linux> 用 `sudo apt install texlive-xetex`  

---

### 💎 **完整命令示例**

   ```bash
   pandoc chapter1.md chapter2.md chapter3.md -o ebook.pdf \
     --toc \
     -N \
     --pdf-engine=xelatex \
     -V CJKmainfont="Microsoft YaHei" \
     -V geometry:"top=2cm, bottom=2cm, left=2cm, right=2cm" \
     --metadata title="我的电子书" \
     --metadata author="作者"
   ```

按此流程操作，即可生成结构清晰、排版专业的电子书 PDF，适用于技术文档、教程或出版级内容。
