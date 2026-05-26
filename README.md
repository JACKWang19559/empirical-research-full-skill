# 实证研究全流程 Skill (empirical-research-full-skill)

从零到一完成中文实证学术论文的完整工作流。

## 工具链

### MCP Servers

| MCP | 用途 | 配置 |
|-----|------|------|
| **Zotero** | 文献管理、PDF标注、BibTeX导出 | `http://localhost:23120/mcp` (需 Zotero 运行) |

### CLI 工具

| 工具 | 用途 | 安装 |
|------|------|------|
| **Playwright CLI** | 浏览器自动化（WoS/CNKI/百度搜索） | `npm install -g @playwright/cli@latest` |
| **PyStata** | Stata 实证分析 | 已安装 E:/stata 18/ |

### Skills

#### 文献搜索与管理

| Skill | 用途 | 工具 |
|-------|------|------|
| `cnki-search` | CNKI 文献检索 | Playwright CLI |
| `cnki-parse-results` | CNKI 结果解析 | Playwright CLI |
| `cnki-paper-detail` | CNKI 论文详情 | Playwright CLI |
| `cnki-download` | CNKI PDF下载 | Playwright CLI |
| `cnki-export` | CNKI 导出到 Zotero | Playwright CLI + Zotero MCP |
| `cnki-journal-search` | CNKI 期刊搜索 | Playwright CLI |
| `cnki-journal-toc` | CNKI 期刊目录 | Playwright CLI |
| `cnki-journal-index` | CNKI 期刊索引查询 | Playwright CLI |
| `cnki-advanced-search` | CNKI 高级检索 | Playwright CLI |
| `cnki-navigate-pages` | CNKI 翻页 | Playwright CLI |
| `wos-search` | WoS 文献检索 | Playwright CLI |
| `wos-parse-results` | WoS 结果解析 | Playwright CLI |
| `wos-paper-detail` | WoS 论文详情 | Playwright CLI |
| `wos-download` | WoS PDF下载 | Playwright CLI |
| `wos-export` | WoS 导出到 Zotero | Playwright CLI + Zotero MCP |
| `wos-navigate-pages` | WoS 翻页 | Playwright CLI |
| `wos-playwright-cli` | WoS Playwright 专用流程 | Playwright CLI |
| `zotero-mcp-guide` | Zotero MCP 使用指南 | Zotero MCP |

#### 数据获取

| Skill | 用途 | 工具 |
|-------|------|------|
| `csmar` | CSMAR 国泰安数据库（查询/下载） | Python SDK |

#### 实证分析

| Skill | 用途 | 工具 |
|-------|------|------|
| `stata-cli` | Stata 命令执行 | PyStata |
| `causal-inference-mixtape` | DID/IV/PSM/RDD 代码模板 | Stata |
| `full-empirical-analysis-stata` | 完整实证分析流程 | Stata |

#### 论文写作

| Skill | 用途 |
|-------|------|
| `introduction-writing-guide` | 引言写作 |
| `literature-review-writing` | 文献综述写作 |
| `methods-section-guide` | 研究设计写作 |
| `discussion-writing-guide` | 讨论与结论写作 |
| `abstract-writing-guide` | 摘要写作 |
| `academic-writing-refiner` | 学术英语润色 |
| `academic-writing-latex` | LaTeX 学术写作 |
| `docx` | Word 文档操作 |
| `response-to-reviewers` | 审稿意见回复 |
| `bibliography-management-guide` | 参考文献管理 |

## 工作流概览

```
阶段1: 选题
  问关键词 → 百度搜政策 + CNKI搜文献 + WoS搜文献 → 3-4个选题建议
          ↓ 用户确认
阶段2: 文献综述
  CNKI + WoS 搜索 → 下载PDF → Zotero管理 → 撰写综述
          ↓ 用户确认
阶段3: 数据获取
  CSMAR 查表/下载 + 手动收集政策变量
          ↓ 用户确认
阶段4: 数据清洗
  Stata .do: 导入 → 合并 → 变量构建 → Winsorize → 筛选
          ↓ 用户确认
阶段5: 实证分析
  基准回归 → 稳健性(PSM/IV) → 机制(中介效应) → 异质性 → .tex表格
          ↓ 用户确认
阶段6: 论文撰写
  引言 → 文献综述 → 研究设计 → 实证分析 → 结论 → 投稿
```

## 项目结构

```
E:/research/{课题名}/
├── data/
│   ├── raw/          # CSMAR原始数据 + 手动数据
│   └── clean/        # 清洗后面板数据 .dta
├── code/
│   └── analysis.do   # 唯一的Stata .do文件
├── output/
│   ├── tables/       # .tex 三线表
│   └── figures/      # 图表
├── paper/
│   ├── main.tex      # 论文正文
│   └── references.bib # Zotero导出的参考文献
└── README.md         # 课题说明
```

## 表格规范

输出《管理世界》期刊风格三线表：
- 系数 + 星号 (*** 1%, ** 5%, * 10%)
- 括号内聚类标准误
- 固定效应 Yes/No
- Observations + Adjusted R²
