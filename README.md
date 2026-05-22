<!--
  Этот проект создан YingyanChen. Коммерческое использование запрещено.
  该项目由YingyanChen生成，禁止商用。
  This project was created by YingyanChen. Commercial use is prohibited.
-->

# readpaper-skills

[ReadPaper](https://github.com/CrisChenYingyan/ReadPaper) 项目的配套 Claude Code Skill 插件包。
包含 4 个可独立使用的 Skill，用于学术论文的 PDF 转换与 AI 深度阅读分析。

## 包含 Skill

| 命令 | 功能 |
|------|------|
| `/驱动MinerU执行PDF→MD转换` | 本地 MinerU CLI 将 PDF 转为 Markdown |
| `/在线MinerU执行PDF→MD转换` | 通过 mineru.net 在线转换 PDF，自动上传与下载 |
| `/阅读并总结文献` | AI 深度分析单篇 MD，生成结构化 DOCX 阅读笔记 |
| `/批量阅读并总结文献` | 批量处理目录下所有 MD 文件 |

## 安装

### 方法一：Windows PowerShell

```powershell
git clone https://github.com/CrisChenYingyan/readpaper-skills.git
# 将 skills 复制到你的项目
xcopy readpaper-skills\.claude\skills\ <your-project>\.claude\skills\ /E /I /Y
```

### 方法二：macOS / Linux

```bash
git clone https://github.com/CrisChenYingyan/readpaper-skills.git
cp -r readpaper-skills/.claude/skills/ <your-project>/.claude/skills/
```

安装后，在 Claude Code 中打开你的项目目录，即可使用以上 `/` 命令。

## 依赖

| 依赖 | 说明 |
|------|------|
| [Claude Code](https://claude.ai/code) | 必须 |
| Python 3.8+（加入 PATH） | `/阅读并总结文献` 和 `/在线MinerU` 需要 |
| python-docx | `pip install python-docx` |
| MinerU CLI（可选） | 本地转换：`pip install magic-pdf[full]` |
| mineru.net 账号（可选） | 在线转换时需要 |
| Claude in Chrome MCP（可选） | `/在线MinerU执行PDF→MD转换` 需要浏览器控制 |

## 项目目录要求

Skill 运行时会自动以当前工作目录（即包含 `CLAUDE.md` 的目录）作为项目根目录，
并自动探测系统 `python`。你的项目需包含以下目录：

```
your-project/
├── pdf_unread/    # 放入待转换的 PDF
├── pdf_done/      # PDF 归档（自动创建或手动创建）
├── md_unread/     # MinerU 输出的 MD
├── md_done/       # 分析后 MD 归档
├── docx_done/     # DOCX 阅读笔记输出
└── scripts/
    ├── generate_reading_notes.py   # 来自主项目
    └── cors_server.py              # 来自主项目（在线版需要）
```

推荐直接使用 [ReadPaper](https://github.com/CrisChenYingyan/ReadPaper) 主项目，已包含所有脚本和目录结构。

## License

本项目仅供学习与个人研究使用，禁止商业用途。
