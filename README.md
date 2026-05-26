# 🎓 Empirical Research Full Skill

> **从零到一的中文实证学术论文全流程 AI 工作流**
>
> 一个为 Hermes Agent 设计的 Skill，集成文献检索、数据获取、计量分析、论文撰写六大阶段，覆盖 DID/IV/PSM/FE 等主流方法论，输出《管理世界》期刊风格的三线表。

## ✨ 特性

- 🔄 **全流程覆盖**：选题 → 文献综述 → 数据获取 → 数据清洗 → 实证分析 → 论文撰写
- 🤖 **AI 驱动**：每步与用户确认后自动执行，无需手动切换工具
- 📊 **主流方法论**：面板 FE、DID、IV/2SLS、PSM、中介效应、异质性分析
- 📝 **期刊规范**：输出《管理世界》三线表格式，Stata esttab 模板即用
- 🔗 **工具链集成**：CNKI + WoS + Zotero + CSMAR + Stata + LaTeX 全打通

## 🛠 工具链

### 核心依赖

| 工具 | 用途 | 安装 |
|------|------|------|
| [Hermes Agent](https://github.com/NousResearch/hermes-agent) | AI Agent 框架 | 见官方文档 |
| [Playwright CLI](https://github.com/microsoft/playwright-cli) | 浏览器自动化（CNKI/WoS/百度） | `npm install -g @playwright/cli@latest` |
| [Stata 18](https://www.stata.com/) | 计量经济学分析 | 商业授权 |
| [CSMAR](https://cn.gtadata.com) | 企业微观数据 | 个人注册账号 |

### MCP Servers

| MCP | 用途 | 配置 |
|-----|------|------|
| [Zotero MCP](https://github.com/nicholasgasior/zotero-mcp) | 文献管理、PDF 标注 | `http://localhost:23120/mcp` |

### 依赖 Skills（共 22 个）

<details>
<summary>📚 文献搜索与管理（18 个）</summary>

| Skill | 用途 |
|-------|------|
| `cnki-search` | CNKI 文献检索 |
| `cnki-parse-results` | CNKI 结果解析 |
| `cnki-paper-detail` | CNKI 论文详情 |
| `cnki-download` | CNKI PDF 下载 |
| `cnki-export` | CNKI 导出到 Zotero |
| `cnki-journal-search` | CNKI 期刊搜索 |
| `cnki-journal-toc` | CNKI 期刊目录 |
| `cnki-journal-index` | CNKI 期刊索引查询 |
| `cnki-advanced-search` | CNKI 高级检索 |
| `cnki-navigate-pages` | CNKI 翻页 |
| `wos-search` | WoS 文献检索 |
| `wos-parse-results` | WoS 结果解析 |
| `wos-paper-detail` | WoS 论文详情 |
| `wos-download` | WoS PDF 下载 |
| `wos-export` | WoS 导出到 Zotero |
| `wos-navigate-pages` | WoS 翻页 |
| `wos-playwright-cli` | WoS Playwright 专用流程 |
| `zotero-mcp-guide` | Zotero MCP 使用指南 |

</details>

<details>
<summary>📈 数据与分析（3 个）</summary>

| Skill | 用途 |
|-------|------|
| `csmar` | CSMAR 国泰安数据库查询与下载 |
| `stata-cli` | Stata 命令执行（PyStata） |
| `causal-inference-mixtape` | DID/IV/PSM/RDD 代码模板 |

</details>

<details>
<summary>✍️ 论文写作（10 个）</summary>

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

</details>

## 🚀 快速开始

### 1. 安装前置工具

```bash
# Playwright CLI（浏览器自动化）
npm install -g @playwright/cli@latest

# PyStata（需要 Stata 18 已安装）
# 配置环境变量
export STATA_PATH="E:/stata 18/"
```

### 2. 安装 Skill

```bash
# 克隆仓库
git clone https://github.com/your-username/empirical-research-full-skill.git

# 复制到 Hermes skills 目录
cp -r empirical-research-full-skill ~/.hermes/skills/research/

# 或通过 Hermes CLI 安装
hermes skills install ./empirical-research-full-skill
```

### 3. 启动使用

在 Hermes Agent 中说：

```
开始实证研究
```

AI 会引导你完成全流程：

```
AI: 请问您感兴趣的研究方向/关键词是什么？
用户: 跨境电商

AI: 正在并行搜索...
  ├─ 百度：跨境电商综试区 政策
  ├─ CNKI：跨境电商 最新文献
  └─ WoS：cross-border e-commerce

AI: 为您推荐以下选题：
  选题1: 跨境电商综试区对企业全要素生产率的影响
    - 政策背景：2015年起分批设立的综试区（天然DID）
    - 方法：多期DID + 中介效应
    - 机制：降低贸易成本 → 提升TFP
  选题2: ...
  选题3: ...
  选题4: ...

用户: 选题1

AI: 好的，进入文献综述阶段...
```

## 📋 工作流详解

```
┌─────────────────────────────────────────────────────────┐
│  阶段1: 选题                                             │
│  问关键词 → 百度政策 + CNKI文献 + WoS文献 → 3-4选题建议  │
├─────────────────────────────────────────────────────────┤
│  阶段2: 文献综述                                         │
│  CNKI + WoS 搜索 → 下载PDF → Zotero管理 → 撰写综述      │
├─────────────────────────────────────────────────────────┤
│  阶段3: 数据获取                                         │
│  CSMAR 查表/字段 → 条件查询 → 打包下载                   │
├─────────────────────────────────────────────────────────┤
│  阶段4: 数据清洗                                         │
│  Stata .do: 导入→合并→变量构建→Winsorize→样本筛选        │
├─────────────────────────────────────────────────────────┤
│  阶段5: 实证分析                                         │
│  基准回归 → 稳健性(PSM/IV) → 机制(中介) → 异质性         │
│  → 自动输出 .tex 三线表                                  │
├─────────────────────────────────────────────────────────┤
│  阶段6: 论文撰写                                         │
│  引言→文献综述→研究设计→实证分析→结论→参考文献           │
└─────────────────────────────────────────────────────────┘
```

## 📁 项目结构

每个研究课题生成独立文件夹：

```
research/{课题名}/
├── data/
│   ├── raw/              # CSMAR 原始数据 + 手动数据
│   └── clean/            # 清洗后面板数据 (.dta)
├── code/
│   └── analysis.do       # 唯一的 Stata .do 文件
├── output/
│   ├── tables/           # .tex 三线表
│   │   ├── descriptive.tex
│   │   ├── baseline.tex
│   │   ├── robustness.tex
│   │   ├── mediation.tex
│   │   └── heterogeneity.tex
│   └── figures/          # 图表
├── paper/
│   ├── main.tex          # 论文正文 (LaTeX)
│   └── references.bib    # Zotero 导出的参考文献
└── README.md             # 课题说明
```

## 📊 表格规范

输出《管理世界》期刊风格三线表：

```latex
% Stata esttab 输出模板
esttab using "output/tables/baseline.tex", ///
    b(3) se(3) star(* 0.1 ** 0.05 *** 0.01) ///
    indicate("Controls = *.控变量" "Year FE = *.year" "Ind FE = *.ind") ///
    r2 ar2 N ///
    booktabs fragment nomtitles
```

效果：

```
变量              (1)           (2)           (3)
                无控制变量    添加控制变量    PSM后回归
─────────────────────────────────────────────────────
AI指标          0.090***      0.038**       0.035**
               (0.014)       (0.015)       (0.014)
Controls         No            Yes           Yes
Year FE          Yes           Yes           Yes
Ind FE           Yes           Yes           Yes
─────────────────────────────────────────────────────
Observations    18106         18106          6390
Adjusted R²     0.309         0.505          0.540
─────────────────────────────────────────────────────
注：***、**、*分别表示在1%、5%、10%水平下显著，
括号内为行业层面的聚类标准误。
```

## 🔧 方法论覆盖

| 方法 | Stata 命令 | 适用场景 |
|------|-----------|----------|
| 面板固定效应 | `reghdfe` | 基准回归 |
| 多期 DID | `reghdfe` + 交互项 | 政策评估 |
| 工具变量 (2SLS) | `ivreghdfe` | 内生性处理 |
| 倾向得分匹配 | `psmatch2` | 自选择偏差 |
| 中介效应 | 三步法 | 机制分析 |
| 异质性分析 | 分组回归 | 调节效应 |

> 💡 后续根据实际使用持续迭代，扩展 RDD、合成控制等方法。

## ⚠️ 注意事项

- **CSMAR 限制**：同一查询条件 30 分钟内只能执行一次
- **浏览器搜索**：国内环境优先走百度（`playwright-cli open "https://www.baidu.com"`）
- **WoS 检测**：必须用 Playwright CLI，Chrome DevTools MCP 会被反自动化拦截
- **Stata 版本**：需要 Stata 18 MP，PyStata 需正确配置 `SYSDIR_STATA`
- **每步确认**：每个阶段完成后 AI 会询问用户确认，再进入下一阶段

## 📄 License

MIT

## 🙏 致谢

- [Hermes Agent](https://github.com/NousResearch/hermes-agent) — AI Agent 框架
- [Playwright CLI](https://github.com/microsoft/playwright-cli) — 浏览器自动化
- [CSMAR](https://cn.gtadata.com) — 企业微观数据
- [Zotero](https://www.zotero.org/) — 文献管理
- 《管理世界》— 论文格式参考
