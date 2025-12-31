# Manus × Claude Code 协同工作指南

## 🎯 协同目标

实现Manus（策略设计）与Claude Code（本地执行）的无缝协同，通过GitHub仓库进行工作流版本管理和迭代优化。

---

## 🏗️ 协同架构

```
┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│    Manus     │         │    GitHub    │         │ Claude Code  │
│  (策略设计)   │ ──推送──→│  (版本管理)   │ ──拉取──→│  (本地执行)   │
│              │         │              │         │              │
│ - 需求分析    │         │ workflow-    │         │ - 导入工作流  │
│ - 工作流设计  │         │ only分支     │         │ - 配置环境    │
│ - JSON生成   │         │              │         │ - 测试运行    │
│ - 文档编写    │         │ - 工作流文件  │         │ - 反馈问题    │
└──────────────┘         │ - 文档       │         └──────────────┘
                         │ - 变更记录    │
                         └──────────────┘
                                │
                                │ 反馈循环
                                ↓
                         ┌──────────────┐
                         │     用户      │
                         │  (协调决策)   │
                         │              │
                         │ - 需求确认    │
                         │ - 内容审核    │
                         │ - 最终决策    │
                         └──────────────┘
```

---

## 📋 角色分工

### Manus的职责

✅ **策略层面**：
- 分析营销需求
- 设计工作流逻辑
- 规划节点流程
- 优化AI Prompt

✅ **技术层面**：
- 生成n8n工作流JSON
- 编写技术文档
- 设计数据结构
- 提供故障排查方案

✅ **版本管理**：
- 提交到GitHub
- 编写commit message
- 维护CHANGELOG
- 管理版本迭代

### Claude Code的职责

✅ **环境管理**：
- 部署本地n8n
- 配置Docker
- 管理环境变量
- 维护服务运行

✅ **工作流操作**：
- 从GitHub拉取更新
- 导入工作流JSON
- 连接凭证和节点
- 测试工作流运行

✅ **问题反馈**：
- 报告错误日志
- 描述问题现象
- 提供执行截图
- 建议改进方向

### 用户的职责

✅ **需求管理**：
- 明确营销目标
- 确认功能需求
- 决定优先级
- 验收最终效果

✅ **内容管理**：
- 审核AI生成的内容
- 修改优化文案
- 决定发布时机
- 监测数据表现

✅ **协调沟通**：
- 在Manus和Claude Code之间传递信息
- 确认技术方案
- 决策重要变更

---

## 🔄 协同流程

### 阶段1：需求分析

**用户 → Manus**：
```
"我需要一个自动生成小红书内容的工作流"
```

**Manus的工作**：
1. 理解需求细节
2. 分析现有工作流结构
3. 设计新工作流逻辑
4. 规划节点和数据流

**输出**：
- 工作流设计文档
- 节点流程图
- 数据结构定义

---

### 阶段2：工作流开发

**Manus的工作**：
1. 编写工作流JSON文件
2. 设计AI Prompt
3. 配置节点参数
4. 编写README文档

**输出**：
- `mbti-content-creator.json`
- `README.md`
- `COLLABORATION_GUIDE.md`

---

### 阶段3：提交到GitHub

**Manus → GitHub**：
```bash
# Manus会生成这些文件
workflows/
├── mbti-content-creator.json
├── README.md
├── COLLABORATION_GUIDE.md
└── CHANGELOG.md

# 并提交commit
git add workflows/
git commit -m "feat: 新增MBTI内容创作工作流

- 自动从素材库和产品库读取数据
- AI生成小红书和公众号内容
- 写入创作库等待审核
- 周一三五9点自动执行"

git push origin workflow-only
```

---

### 阶段4：通知Claude Code

**用户 → Claude Code**：
```
"Manus已经更新了workflow-only分支，
请拉取最新代码并导入mbti-content-creator.json工作流"
```

---

### 阶段5：Claude Code执行

**Claude Code的操作**：

#### 步骤1：拉取最新代码
```bash
cd /path/to/n8n
git fetch origin
git checkout workflow-only
git pull origin workflow-only
```

#### 步骤2：查看新文件
```bash
ls workflows/
# 应该看到：
# mbti-content-creator.json
# README.md
# COLLABORATION_GUIDE.md
```

#### 步骤3：在n8n中导入工作流

1. 打开n8n界面（http://localhost:5678）
2. 点击右上角"+" → "Import from File"
3. 选择`workflows/mbti-content-creator.json`
4. 点击"Import"

#### 步骤4：配置环境变量

在n8n Settings → Environment Variables中添加：
```bash
NOTION_MATERIAL_DATABASE_ID=xxx
NOTION_PRODUCT_DATABASE_ID=xxx
NOTION_CREATION_DATABASE_ID=xxx
AI_API_BASE_URL=http://localhost:8045
OPENAI_API_KEY=sk-xxx
```

#### 步骤5：测试运行

1. 点击工作流中的"手动触发"节点
2. 点击"Execute Workflow"
3. 查看执行结果
4. 检查Notion创作库是否有新内容

---

### 阶段6：反馈和优化

**Claude Code → 用户 → Manus**：

#### 成功场景
```
"工作流运行成功！
- 生成了1篇小红书内容
- 生成了1篇公众号内容
- 已写入Notion创作库

建议：
- 小红书标题可以更吸引人
- 公众号内容可以更深入"
```

#### 失败场景
```
"工作流运行失败，错误信息：
Error in node 'AI生成小红书标题': 
Request failed with status code 401

可能原因：
- AI API密钥无效
- API地址错误"
```

**Manus的优化**：
1. 分析问题原因
2. 修改工作流配置
3. 优化AI Prompt
4. 更新JSON文件
5. 提交新版本到GitHub

---

## 📁 GitHub仓库结构

```
workflow-only/
├── workflows/
│   ├── mbti-rss-collector.json          # V1: RSS采集
│   ├── mbti-rss-collector-v2.json       # V2: 智能采集
│   ├── mbti-content-creator.json        # 内容创作
│   ├── mbti-content-publisher.json      # 内容发布（未来）
│   ├── mbti-data-monitor.json           # 数据监测（未来）
│   ├── README.md                        # 项目说明
│   ├── COLLABORATION_GUIDE.md           # 本文档
│   └── CHANGELOG.md                     # 变更记录
└── docs/
    ├── optimization_plan.md             # 优化方案
    ├── workflow_analysis.md             # 工作流分析
    └── marketing_strategy.md            # 营销策略
```

---

## 🔧 常见问题

### Q1: 如何查看GitHub上的最新更新？

**Claude Code操作**：
```bash
git log --oneline -10
# 查看最近10次提交

git show HEAD
# 查看最新提交的详细内容

git diff HEAD~1 HEAD
# 对比最新提交和上一次提交的差异
```

---

### Q2: 导入工作流后节点显示错误？

**可能原因**：
- 环境变量未配置
- 节点引用的凭证不存在
- n8n版本不兼容

**解决方案**：
1. 检查所有环境变量是否已配置
2. 在n8n中创建必要的凭证（Notion、OpenAI等）
3. 手动连接节点和凭证
4. 更新n8n到最新版本

---

### Q3: 工作流运行失败如何调试？

**Claude Code操作**：
1. 点击失败的节点
2. 查看"Executions"标签
3. 查看错误信息和输入输出数据
4. 截图发送给用户
5. 用户转述给Manus

**Manus需要的信息**：
- 错误节点名称
- 完整错误信息
- 节点输入数据
- 执行时间
- 环境变量配置（脱敏）

---

### Q4: 如何回滚到之前的版本？

**Claude Code操作**：
```bash
# 查看提交历史
git log --oneline

# 回滚到指定版本
git checkout <commit-hash> workflows/mbti-content-creator.json

# 在n8n中重新导入旧版本
```

---

### Q5: 如何提出改进建议？

**Claude Code → 用户 → Manus**：

**格式**：
```
【建议类型】功能优化 / Bug修复 / 性能提升

【当前问题】
描述现状和不足

【期望效果】
描述理想状态

【建议方案】（可选）
提出具体改进思路

【优先级】高 / 中 / 低
```

**示例**：
```
【建议类型】功能优化

【当前问题】
AI生成的小红书标题有时太长，超过25字

【期望效果】
标题控制在15-20字，更符合小红书规范

【建议方案】
在AI Prompt中强调字数限制，
并在整合节点中添加字数检查和截断

【优先级】中
```

---

## 🚀 最佳实践

### 1. 版本命名规范

**工作流文件**：
- 新功能：`mbti-xxx.json`
- 迭代版本：`mbti-xxx-v2.json`, `mbti-xxx-v3.json`
- 实验性：`mbti-xxx-experimental.json`

**Git Commit**：
- 新增功能：`feat: 新增XXX工作流`
- Bug修复：`fix: 修复XXX节点错误`
- 优化改进：`refactor: 优化XXX逻辑`
- 文档更新：`docs: 更新XXX文档`

### 2. 测试流程

**在生产环境启用前**：
1. 手动触发测试3次
2. 检查所有输出数据
3. 验证Notion写入正确
4. 确认无错误日志
5. 审核AI生成的内容质量

### 3. 环境变量管理

**安全原则**：
- ✅ 使用环境变量存储密钥
- ✅ 不要在JSON中硬编码密钥
- ✅ 不要将密钥提交到GitHub
- ✅ 定期轮换API密钥

**备份**：
```bash
# Claude Code本地保存环境变量备份
cat > .env.backup <<EOF
NOTION_API_TOKEN=secret_xxx
OPENAI_API_KEY=sk-xxx
...
EOF
```

### 4. 文档同步

**Manus更新文档后**：
- README.md：项目说明
- CHANGELOG.md：版本记录
- COLLABORATION_GUIDE.md：协同指南

**Claude Code应该**：
1. 拉取最新文档
2. 阅读变更内容
3. 理解新功能
4. 按文档操作

---

## 📊 协同效率指标

### 目标

| 指标 | 目标值 | 说明 |
|-----|--------|------|
| 需求响应时间 | < 2小时 | Manus收到需求到开始设计 |
| 工作流开发时间 | < 4小时 | 设计到提交GitHub |
| 导入测试时间 | < 30分钟 | Claude Code导入到测试完成 |
| 问题修复时间 | < 1小时 | 发现问题到提交修复 |
| 迭代周期 | 1-2天 | 从需求到上线 |

### 当前状态

- ✅ 素材采集工作流：已上线
- ✅ 内容创作工作流：开发完成，待测试
- 🔄 内容发布工作流：设计中
- 📅 数据监测工作流：计划中

---

## 🎯 下一步计划

### 短期（1周内）
1. 测试`mbti-content-creator.json`
2. 优化AI Prompt
3. 完善错误处理
4. 补充使用文档

### 中期（2-4周）
1. 开发`mbti-content-publisher.json`
2. 对接小红书API
3. 对接公众号API
4. 实现自动发布

### 长期（1-3个月）
1. 开发`mbti-data-monitor.json`
2. 自动数据分析
3. 高表现内容识别
4. 营销策略优化

---

## 📞 联系方式

**Manus**：通过用户转述  
**Claude Code**：本地AI助手  
**用户**：协调者

**沟通渠道**：
- 需求和反馈：用户 ↔ Manus
- 技术执行：用户 ↔ Claude Code
- 版本管理：GitHub workflow-only分支

---

## 📝 更新日志

| 日期 | 版本 | 更新内容 |
|-----|------|---------|
| 2025-12-31 | v1.0 | 初始版本，定义协同流程 |

---

**让我们一起打造高效的营销自动化系统！** 🚀
