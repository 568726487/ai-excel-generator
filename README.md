# AI Excel Generator - GitHub Action

用 AI 自然语言生成 Excel 表格的 GitHub Action 自动化工具。

## 效果示例

在 GitHub 上手动触发，输入一句话：
> "做一个季度销售报表，包含产品名称、Q1/Q2/Q3/Q4 销售额、年度合计，5 个产品，最后一行用 SUM 公式计算各季度总计"

自动生成一个带格式、带公式、可直接打开的 `.xlsx` 文件，作为 Artifact 下载。

---

## 技术栈

| 组件 | 作用 |
|------|------|
| **GitHub Models** (GPT-4.1-mini) | 解析自然语言需求，生成 OfficeCLI 指令 |
| **OfficeCLI** | 实际创建/写入 Excel 文件，内置 350+ 函数引擎 |
| **GitHub Actions** | 自动化运行 + 结果上传 |

---

## 部署步骤（3 步搞定）

### 第 1 步：创建一个新仓库

去 GitHub 新建一个仓库（可以是 Public 或 Private）。

### 第 2 步：添加 workflow 文件

把 `.github/workflows/generate-excel.yml` 这个文件放到你仓库的 `.github/workflows/` 目录下。

最简单的方式：
1. 打开你的 GitHub 仓库
2. 点 `Add file` → `Create new file`
3. 文件名输入：`.github/workflows/generate-excel.yml`
4. 把本项目里的 yml 文件内容粘贴进去
5. 点 `Commit changes`

### 第 3 步：启用并使用

1. 打开仓库的 **Actions** 标签页
2. 左侧找到 **AI Excel Generator**
3. 点 **Run workflow** 按钮
4. 在弹出的输入框里描述你想要的 Excel 表格
5. 点绿色 **Run workflow** 确认

等 30 秒左右，刷新页面，点进去这次运行记录，底部的 **Artifacts** 区域就能下载生成的 Excel 文件了。

---

## 使用示例

| 输入需求 | 生成结果 |
|----------|----------|
| "做一个员工薪资表，包含姓名、部门、职位、基本工资、奖金、社保，10行示例数据，最后一行用SUM公式合计" | 员工薪资.xlsx，表头蓝底白字加粗，末行合计公式 |
| "做一个家庭月度预算表，分类：餐饮、交通、住房、娱乐、其他，列出1月到6月预算和实际支出，最后算差异" | 预算对比表，含公式和分类汇总 |
| "做一个项目进度跟踪表，列：任务名、负责人、开始日期、截止日期、完成状态、进度百分比" | 项目跟踪表，带状态列 |

---

## 自定义修改

### 换更强的模型

编辑 yml 文件里的 `model:` 行，可选模型列表见 [GitHub Models 目录](https://github.com/marketplace/models)：

```yaml
model: openai/gpt-4.1          # 更强
model: openai/gpt-4.1-mini     # 快速便宜（默认）
model: mistral-ai/mistral-large  # 开源模型
model: microsoft/deepseek-r1    # 推理模型
```

### 改触发方式

当前是**手动触发**（workflow_dispatch）。如果你想改成 Issue 评论触发，把 on 部分改成：

```yaml
on:
  issues:
    types: [opened, edited]
  issue_comment:
    types: [created]
```

然后在 step 里读取 issue 内容作为 prompt 即可。

### 加更多 Excel 功能

OfficeCLI 支持的完整能力见 [OfficeCLI 官方文档](https://github.com/iOfficeAI/OfficeCLI)：
- 图表（柱状图/折线图/饼图）
- 数据透视表
- 条件格式
- 数据验证下拉菜单
- 单元格合并、边框、对齐

把这些命令加到 system-prompt 的说明里，AI 就能自动用上了。

---

## 注意事项

1. **GitHub Models 免费额度**：公共仓库 Actions 分钟数免费，GitHub Models 有每月免费额度，个人开发者够用
2. **Private 仓库**：Actions 会消耗你的分钟数额度，Private 仓库需要注意用量
3. **文件大小**：单个 Artifact 最大 100MB，生成普通表格完全够用
4. **文件保留**：默认保留 30 天，之后自动清理

---

## 参考项目

- [OfficeCLI](https://github.com/iOfficeAI/OfficeCLI) - 33k Stars，AI 原生 Office 工具
- [GitHub Models in Actions](https://github.blog/ai-and-ml/generative-ai/automate-your-project-with-github-models-in-actions/) - 官方教程
