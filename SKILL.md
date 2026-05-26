---
name: empirical-research-full-skill
description: >-
  从零到一完成中文实证学术论文的全流程指导。覆盖选题、文献综述、数据获取、
  数据清洗、实证分析、论文撰写六大阶段。每步与用户确认后再继续。
  工具链：CNKI + WoS (Playwright CLI) + Zotero (MCP) + CSMAR (Skill) + Stata (PyStata) + LaTeX/docx。
  输出《管理世界》期刊风格的三线表和学术论文。
triggers:
  - 实证研究
  - 实证论文
  - empirical research
  - 学术论文
  - 论文写作
  - 数据分析
  - 从零开始写论文
---

# 实证研究全流程 Skill

## 概述

本 Skill 指导从选题到投稿的完整中文实证学术论文流程。
采用**每步确认**模式——每个阶段完成后向用户确认，再进入下一阶段。
方法论覆盖最常用的面板OLS/FE、DID、IV/2SLS、PSM、中介效应，后续根据实践持续迭代。

## 核心原则

1. **中国现实问题 + 政策冲击 + 微观机制** 的选题范式
2. 每步与用户确认后再继续
3. Stata 一个大 .do 文件跑到底
4. 表格输出《管理世界》期刊风格（三线表）
5. 浏览器搜索优先走百度（Playwright → baidu.com）

## 六大阶段

---

### 阶段 1：选题

**目标：** 确定研究课题

**步骤：**

1. 向用户询问感兴趣的**关键词/方向**（如"跨境电商"、"人工智能"、"碳排放"）
2. **并行搜索**三条线索：
   - **政策搜索**（Playwright → 百度）：搜索该关键词 + "政策"，了解中国现实政策背景和政策冲击
   - **中文文献趋势**（Playwright → CNKI）：搜索该关键词，按发表时间排序，看近2-3年研究热点和方法
   - **英文文献趋势**（Playwright CLI → WoS）：搜索英文关键词，按时间排序，看国际前沿方向
3. 综合政策背景 + 中英文文献趋势，向用户呈现 3-4 个选题建议，格式：
   ```
   选题建议 N：
   - 研究问题：XXX如何影响YYY？
   - 政策背景：ZZZ政策（年份）提供了准自然实验
   - 文献支撑：CNKI/WoS近期有哪些相关研究，gap在哪里
   - 核心方法：DID/IV/面板FE
   - 微观机制：通过AAA渠道影响BBB
   ```
4. 用户确认选题后，确定：
   - 研究问题
   - 核心解释变量、被解释变量、控制变量
   - 识别策略（DID/IV/PSM/FE）
   - 数据来源（CSMAR为主 + 手动收集政策变量）

**搜索模板：**
```
# 1. 百度搜政策
playwright-cli open "https://www.baidu.com" --headed
playwright-cli fill <ref> "跨境电商综试区 政策 效果"
playwright-cli click <search-btn>

# 2. CNKI 搜中文文献趋势（用 cnki-search skill）
# 按时间排序，看近2-3年发表的高频主题

# 3. WoS 搜英文文献趋势（用 wos-search skill）
# 按 date-descending 排序，看国际前沿
```

---

### 阶段 2：文献综述

**目标：** 建立文献基础，输出文献综述初稿

**步骤：**

1. **中文文献搜索**（CNKI via Playwright）：
   - 搜索核心关键词，提取 Top 20 高引文献
   - 按被引量排序
   - 记录：题名、作者、期刊、年份、被引量、核心结论

2. **英文文献搜索**（WoS via Playwright CLI）：
   - 翻译中文关键词为英文
   - 用 `playwright-cli eval` 调用 WoS API
   - 按被引量排序，提取 Top 15
   - 记录：title, authors, source, year, citations, DOI

3. **文献管理**（Zotero MCP）：
   - 选择与课题最相关的 10-15 篇文献
   - 下载 PDF（通过 CNKI/WoS skill）
   - 通过 Zotero MCP 的 `mcp_zotero_write_item` 创建条目
   - 创建专题 Collection

4. **综述撰写**：
   - 参考 `literature-review-writing` skill
   - 按主题分类：概念界定 → 理论基础 → 实证研究 → 文献述评
   - 引出研究假设

5. **向用户确认**：综述结构和研究假设

---

### 阶段 3：数据获取

**目标：** 获取并整合分析所需数据

**步骤：**

1. **确定变量清单**，格式：
   | 变量 | 符号 | 定义 | 数据来源 | 表名 |
   |------|------|------|----------|------|

2. **CSMAR 数据获取**：
   ```bash
   # 查数据库
   python "$CSMAR_SCRIPT" csmar_get_dbs '{}'
   # 查表
   python "$CSMAR_SCRIPT" csmar_get_tables '{"database_name": "财务报表"}'
   # 查字段
   python "$CSMAR_SCRIPT" csmar_get_fields '{"table_name": "FS_Combas"}'
   # 下载数据
   python "$CSMAR_SCRIPT" csmar_download '{...}'
   # 解压
   python "$CSMAR_SCRIPT" csmar_unzip '{"file_path": "~/csmardata/zip/xxx.zip"}'
   ```

3. **手动数据收集**（政策变量等）：
   - 用 Playwright + 百度搜索政策文件
   - 整理为 CSV

4. **数据存放**：`{项目目录}/data/raw/`

5. **向用户确认**：数据获取情况，是否需要补充

---

### 阶段 4：数据清洗与变量构建

**目标：** 生成分析用面板数据

**Stata .do 文件结构（一个大文件）：**
```stata
/*=====================================
  项目名称：XXX
  创建日期：YYYY-MM-DD
  作者：XXX
======================================*/

// ---- 0. 全局设置 ----
clear all
set more off
cd "E:/research/课题名"

// ---- 1. 数据导入与合并 ----
use "data/raw/xxx.dta", clear
merge 1:1 Stkcd Accper using "data/raw/yyy.dta"
keep if _merge == 3
drop _merge

// ---- 2. 变量构建 ----
// 被解释变量
gen ln_tfp = ln(TFP)
// 解释变量
gen treat_post = treat * post
// 控制变量
gen ln_size = ln(Asset)

// ---- 3. 样本筛选 ----
drop if Stkcd == ""  // 去除缺失
drop if year < 2010  // 时间范围

// ---- 4. Winsorize ----
winsor2 ln_tfp ln_size, cuts(1 99) replace

// ---- 5. 描述性统计 ----
estpost summarize ln_tfp treat ln_size age leverage growth, detail
esttab using "output/tables/descriptive_stats.tex", ///
    cells("count mean sd min p25 p50 p75 max") replace

// ---- 6. 相关性分析 ----
pwcorr ln_tfp treat ln_size age leverage, star(0.01)
```

**数据存放：** `{项目目录}/data/clean/`

**向用户确认**：变量构建和样本筛选是否合理

---

### 阶段 5：实证分析

**目标：** 完成全部实证检验，输出 .tex 表格

**Stata 检验模块（继续在同一个 .do 文件中）：**

```stata
// ---- 7. 基准回归 ----
eststo clear
eststo m1: reghdfe ln_tfp treat, absorb(year ind pro) vce(cluster ind)
eststo m2: reghdfe ln_tfp treat ln_size age leverage growth board dual top1, absorb(year ind pro) vce(cluster ind)
esttab m1 m2 using "output/tables/baseline.tex", ///
    b(3) se(3) star(* 0.1 ** 0.05 *** 0.01) ///
    indicate("Controls = ln_size age" "Year FE = *.year" "Ind FE = *.ind" "Pro FE = *.pro") ///
    r2 ar2 N replace

// ---- 8. 稳健性检验 ----
// 8.1 PSM
psmatch2 treat ln_size age leverage growth board dual top1, logit neighbor(1) common
// 8.2 工具变量
ivreghdfe ln_tfp (treat = iv_var) ln_size age leverage, absorb(year ind) cluster(ind)
// 8.3 替换被解释变量
// 8.4 缩尾处理
// 8.5 剔除特殊样本

// ---- 9. 机制分析（中介效应）----
// Step 1: X → Y（已在基准回归）
// Step 2: X → M
eststo med1: reghdfe mediator treat ln_size age leverage, absorb(year ind pro) vce(cluster ind)
// Step 3: X + M → Y
eststo med2: reghdfe ln_tfp treat mediator ln_size age leverage, absorb(year ind pro) vce(cluster ind)

// ---- 10. 异质性分析 ----
// 按产权性质分组
eststo soe: reghdfe ln_tfp treat ln_size age leverage if soe==1, absorb(year ind pro) vce(cluster ind)
eststo nonsoe: reghdfe ln_tfp treat ln_size age leverage if soe==0, absorb(year ind pro) vce(cluster ind)

// ---- 11. 进一步分析（企业价值等）----
```

**关键输出：**
- `output/tables/descriptive_stats.tex` — 描述性统计
- `output/tables/baseline.tex` — 基准回归
- `output/tables/robustness.tex` — 稳健性检验
- `output/tables/mediation.tex` — 机制分析
- `output/tables/heterogeneity.tex` — 异质性分析

**向用户确认**：回归结果是否符合预期，是否需要调整

---

### 阶段 6：论文撰写

**目标：** 完成论文初稿

**论文结构（参考《管理世界》）：**

```
摘要（300字）
关键词（3-5个）
一、引言（问题提出 + 贡献点）
二、文献回顾与研究假说
三、研究设计（数据、变量、模型）
四、实证分析
  （一）描述性统计
  （二）基准回归
  （三）稳健性检验
  （四）机制分析
  （五）异质性分析
五、进一步分析
六、结论与建议
参考文献
```

**表格规范（《管理世界》）：**

三线表格式：
- 系数行：系数值 + 星号（*** 1%, ** 5%, * 10%）
- 标准误行：括号内，系数下方
- 固定效应：Yes/No
- 底部：Observations, Adjusted R²
- 脚注：`注：***、**、*分别表示在1%、5%、10%水平下显著，括号内为行业层面的聚类标准误。`

Stata esttab 模板：
```stata
esttab using "output/tables/xxx.tex", ///
    b(3) se(3) star(* 0.1 ** 0.05 *** 0.01) ///
    indicate("Controls = *.控变量" "Year FE = *.year" "Ind FE = *.ind") ///
    r2 ar2 N ///
    booktabs fragment nomtitles ///
    posthead("\hline \\\\[-1.8ex]") ///
    prefoot("\hline") ///
    postfoot("\hline")
```

**参考 skills：**
- `introduction-writing-guide` — 引言写作
- `literature-review-writing` — 文献综述
- `methods-section-guide` — 研究设计
- `discussion-writing-guide` — 结论讨论
- `abstract-writing-guide` — 摘要
- `academic-writing-refiner` — 学术润色
- `docx` — Word 文档操作
- `response-to-reviewers` — 审稿回复

**向用户确认**：初稿是否满意，需要修改的地方

---

## 项目目录结构

```
E:/research/{课题名}/
├── data/
│   ├── raw/          # CSMAR下载原始数据
│   └── clean/        # 清洗后面板数据
├── code/
│   └── analysis.do   # 唯一的Stata .do文件
├── output/
│   ├── tables/       # .tex 表格文件
│   └── figures/      # 图表
├── paper/
│   ├── main.tex      # 论文正文（LaTeX）
│   └── references.bib # 参考文献（Zotero导出）
└── README.md         # 课题说明
```

## 注意事项

1. **每步确认**：每个阶段结束后必须向用户确认再继续
2. **一个 .do 文件**：所有 Stata 代码在一个文件中，清晰分段注释
3. **浏览器搜索走百度**：`playwright-cli open "https://www.baidu.com" --headed`
4. **CSMAR 查询限制**：同一条件30分钟内只能查一次
5. **Winsorize**：连续变量在1%和99%分位缩尾
6. **聚类标准误**：默认在行业层面聚类 `vce(cluster ind)`
7. **固定效应**：默认控制 年度+行业+省份 `absorb(year ind pro)`
8. **表格输出**：所有表格输出为 .tex 格式，直接嵌入论文
