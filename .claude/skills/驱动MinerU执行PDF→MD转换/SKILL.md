# 驱动MinerU执行PDF→MD转换

当用户输入 `/驱动MinerU执行PDF→MD转换` 时，严格按以下步骤执行。

## 项目根目录

Claude Code 启动时已自动切换至项目根目录（即包含 `CLAUDE.md` 的目录）。
后续所有相对路径均以此为基础。

## 参数解析

从用户消息中提取以下参数（均为可选）：

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `文件名` | 目标 PDF 文件名（可带或不带 `.pdf`） | 来源文件夹中字母顺序第一篇 |
| `--from 路径` | PDF 来源文件夹（绝对路径或相对于项目根目录） | `pdf_unread` |
| `--to 路径` | MD 输出文件夹（绝对路径或相对于项目根目录） | `md_unread` |

PDF 归档目标（固定，不可修改）：项目根目录下的 `pdf_done`

## 执行步骤

### 第 1 步：确定完整路径

- 来源文件夹 A = `--from` 值 或 当前目录下的 `pdf_unread`
- 输出文件夹 B = `--to` 值 或 当前目录下的 `md_unread`
- PDF 归档目标 = 当前目录下的 `pdf_done`

### 第 2 步：找到目标 PDF

使用 Glob 工具列出文件夹 A 中所有 `.pdf` 文件：

- 若用户指定了文件名：找到匹配文件（自动补全 `.pdf` 后缀）
- 若未指定：取字母顺序第一个 `.pdf` 文件
- 若文件夹 A 中没有 PDF：告知用户 "【文件夹A】中没有找到 PDF 文件"，停止执行

### 第 3 步：创建临时输出目录

在文件夹 B 下创建临时目录 `_mineru_temp`：
`【文件夹B】\_mineru_temp`

### 第 4 步：调用 MinerU 执行转换

使用 Bash 工具执行：

```bash
magic-pdf -p "【PDF完整路径】" -o "【文件夹B】\_mineru_temp" -m auto
```

等待命令完成。若报错 `magic-pdf: command not found`，改用：

```bash
mineru -p "【PDF完整路径】" -o "【文件夹B】\_mineru_temp" -m auto
```

若仍报错，告知用户 "MinerU 未安装，请执行：`pip install magic-pdf[full]`"，停止执行。

### 第 5 步：将 MD 文件移到文件夹 B

MinerU 在临时目录下生成子目录结构。使用 Glob 工具递归查找 `_mineru_temp` 下的所有 `.md` 文件：

```
【文件夹B】\_mineru_temp\**\*.md
```

将找到的每个 `.md` 文件移动到文件夹 B 根目录（仅移动文件，不保留子目录结构）。

### 第 6 步：删除临时目录

使用 Bash 工具删除 `_mineru_temp` 目录及其全部内容：

```bash
rm -rf "【文件夹B】/_mineru_temp"
```

### 第 7 步：归档原始 PDF

将 PDF 文件从文件夹 A 移动到 PDF 归档目标（`pdf_done`）。

### 第 8 步：报告结果

向用户输出：

```
✓ 已转换：【PDF文件名】
✓ MD 文件保存至：【MD文件完整路径】
✓ 原始 PDF 已归档至：pdf_done\【PDF文件名】
```
