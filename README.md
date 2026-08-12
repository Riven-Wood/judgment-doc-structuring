# judgment-doc-structuring

裁判文书结构化处理 WorkBuddy Skill —— 将 .doc/.docx 判决书转化为结构化 CSV 数据，支持格式转化、数据清洗、信息抽取、标签统一、质量检验全流程。

## 这是什么

这是一个 [WorkBuddy](https://www.workbuddy.cn) Skill，用于法学实证研究的数据准备阶段。通过对话驱动，引导用户完成从原始裁判文书到可直接导入 Stata/R/SPSS 的结构化 CSV 的完整流水线。

## 核心能力

| 步骤 | 操作 | 说明 |
|------|------|------|
| 1. 格式转化 | .doc/.docx → 干净纯文本 | 去页眉页脚，macOS 用 textutil 零安装 |
| 2. 数据清洗 | 去重 + 完整性检查 + 事件归总ID + 审级标注 | 排除重复和残缺文书 |
| 3. 信息抽取 | Agent 集群并行抽取结构化字段 → CSV | 支持客观/分类/数值/二分类/列表/文本 6 类字段 |
| 4. 标签统一 | 频次统计 → 同义词归并 → 统一标签 | 用户确认映射关系 |
| 5. 质量检验 | 缺失率 + 抽样幻觉检测 + 迭代清零 | 连续两轮零编造才通过 |

## 数据格式规范

- 比例/程度/强度：小数（0-1），如 60% → `0.6`
- 日期：`YYYY-MM-DD`（连字符）
- 二进制：整数 `0` 或 `1`
- 金额：纯数字，不含"元"字，如 `50000`
- 列表：多项用英文分号 `;` 分隔

## 安装

将 `SKILL.md` 放入 `~/.workbuddy/skills/judgment-doc-structuring/` 目录即可。

### 环境依赖

- Python 3.13+（推荐使用 WorkBuddy 受管 Python）
- 依赖包：`pandas openpyxl pyyaml pdfplumber tqdm chardet scipy`
- macOS 自带 `textutil`（无需安装），Windows 可用 `antiword`

```bash
# 创建 venv 并安装依赖
python3 -m venv .venv
source .venv/bin/activate
pip install pandas openpyxl pyyaml pdfplumber tqdm chardet scipy
```

## 使用方式

在 WorkBuddy 中触发即可，支持自然语言：

```
帮我处理这批裁判文书，提取关键信息
```

Skill 会引导你完成：
1. 确认数据位置
2. 设计字段 Schema
3. 确认清洗规则
4. 启动流水线

## 流水线产出

```
output/
├── raw_texts/              # 转化后的纯文本
├── final_labeled_data.csv  # 最终结构化数据（可直接导入 Stata/R/SPSS）
├── quality_report.txt      # 质量检验报告
└── label_mapping.json      # 标签映射表
```

## 与其他 Skill 的关系

| Skill | 覆盖范围 |
|-------|----------|
| `judgment-doc-structuring`（本 Skill） | 数据处理流水线 → CSV |
| `legal-empirical-research` | 实证分析方法论（研究设计→回归→稳健性→机制→讨论） |

典型流程：先用本 Skill 处理文书得到 CSV，再用 `legal-empirical-research` 做实证分析。

## License

MIT
