# 变更记录 (CHANGELOG)

本文档记录MBTI茶饮营销自动化工作流的所有版本变更。

---

## [Unreleased] - 计划中

### 计划新增
- `mbti-content-publisher.json` - 内容发布工作流
- `mbti-data-monitor.json` - 数据监测工作流
- `mbti-material-enhancer.json` - 素材优化工作流

### 计划优化
- 小红书API对接
- 公众号API对接
- 数据分析报告生成

---

## [1.0.0] - 2025-12-31

### 新增

#### mbti-content-creator.json
**功能**：自动生成小红书和公众号营销内容

**特性**：
- ✅ 从Notion素材库读取高质量素材（热度≥8）
- ✅ 从Notion产品库读取4款茶饮产品
- ✅ 随机选择1条素材和1-2款产品
- ✅ AI生成小红书标题（15-25字）
- ✅ AI生成小红书正文（800-1000字）
- ✅ AI生成小红书标签和配图建议
- ✅ AI生成公众号标题（10-30字）
- ✅ AI生成公众号正文（2000-3000字）
- ✅ 自动写入Notion创作库
- ✅ 生成执行摘要报告
- ✅ 周一、三、五 9:00自动执行

**节点数**：17个

**数据流**：
```
素材库(热度≥8) + 产品库 
    → AI生成内容 
    → 创作库(待审核)
```

**AI模型**：
- 默认：gpt-4o-mini
- 可配置：claude-sonnet-4-5-thinking

**容错机制**：
- AI生成节点启用continueOnFail
- Notion写入节点启用continueOnFail
- 生成失败时使用默认内容

**环境变量**：
- `NOTION_MATERIAL_DATABASE_ID` - 素材库ID
- `NOTION_PRODUCT_DATABASE_ID` - 产品库ID
- `NOTION_CREATION_DATABASE_ID` - 创作库ID
- `AI_API_BASE_URL` - AI API地址
- `AI_MODEL` - AI模型名称（可选）
- `OPENAI_API_KEY` - OpenAI API密钥

---

### 文档

#### README.md
**内容**：
- 项目简介
- 工作流清单
- Notion数据库结构
- 环境变量配置
- 快速开始指南
- 执行时间表
- 内容创作策略
- 预期效果
- 故障排查

#### COLLABORATION_GUIDE.md
**内容**：
- 协同架构图
- 角色分工
- 协同流程（6个阶段）
- GitHub仓库结构
- 常见问题解答
- 最佳实践
- 协同效率指标

#### CHANGELOG.md
**内容**：
- 版本记录格式
- 变更分类（新增/优化/修复/废弃）

---

## [0.2.0] - 2025-12-31（用户已有）

### mbti-rss-collector-v2.json

**优化**：
- ✅ 从RSS被动获取 → SerpAPI主动搜索
- ✅ 简单关键词过滤 → AI深度分析
- ✅ RSS摘要 → 完整网页正文
- ✅ 固定评分 → 智能评分和过滤

**新增特性**：
- 使用SerpAPI搜索MBTI话题
- 抓取搜索结果的完整网页
- 使用HTML节点提取正文
- 调用Claude API进行AI分析
- 质量过滤（契合度≥6.0）
- 生成结构化素材数据

**节点数**：11个（vs V1的8个）

**执行时间**：每天9:00

**容错**：
- 网页抓取启用continueOnFail
- AI分析启用continueOnFail
- Notion写入启用continueOnFail

---

## [0.1.0] - 2025-12-31（用户已有）

### mbti-rss-collector.json

**初始版本**：
- ✅ 从知乎热榜RSS获取内容
- ✅ 23个MBTI关键词过滤
- ✅ 简单评分（相关8分，不相关5分）
- ✅ 写入Notion素材库
- ✅ 生成推荐报告

**节点数**：8个

**执行时间**：每12小时

**数据源**：
- 知乎热榜RSS：`http://host.docker.internal:1200/zhihu/hot`

**关键词列表**：
```
mbti, infj, infp, intj, intp, enfj, enfp, entj, entp,
性格, 内向, 外向, 社恐, 社交, 心理, i人, e人, 
人格, 测试, 年轻人, 消费, 生活
```

---

## 版本规范

### 版本号格式
`主版本号.次版本号.修订号`

- **主版本号**：重大功能变更或架构调整
- **次版本号**：新增功能或重要优化
- **修订号**：Bug修复或小优化

### 变更类型

- **新增 (Added)**：新功能、新工作流、新节点
- **优化 (Changed)**：现有功能的改进
- **修复 (Fixed)**：Bug修复
- **废弃 (Deprecated)**：即将移除的功能
- **移除 (Removed)**：已移除的功能
- **安全 (Security)**：安全相关的修复

---

## 提交规范

### Commit Message格式
```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type类型
- `feat`: 新功能
- `fix`: Bug修复
- `docs`: 文档更新
- `style`: 代码格式调整
- `refactor`: 代码重构
- `perf`: 性能优化
- `test`: 测试相关
- `chore`: 构建或辅助工具变更

### 示例
```
feat(content-creator): 新增MBTI内容创作工作流

- 自动从素材库和产品库读取数据
- AI生成小红书和公众号内容
- 写入创作库等待审核
- 周一三五9点自动执行

Closes #1
```

---

## 升级指南

### 从V1升级到V2（素材采集）

**变更**：
- 数据源：RSS → SerpAPI
- 需要新增环境变量：`SERPAPI_KEY`
- AI分析需要配置：`AI_API_BASE_URL`, `OPENAI_API_KEY`

**步骤**：
1. 配置新的环境变量
2. 导入`mbti-rss-collector-v2.json`
3. 测试运行
4. 停用V1工作流
5. 启用V2工作流

**注意**：
- V2生成的素材字段更丰富
- 建议保留V1作为备份

---

### 新增内容创作工作流

**前置条件**：
- 素材库中有状态为"可用"的素材
- 产品库中已添加产品信息
- 创作库数据库已创建

**步骤**：
1. 配置环境变量：
   - `NOTION_MATERIAL_DATABASE_ID`
   - `NOTION_PRODUCT_DATABASE_ID`
   - `NOTION_CREATION_DATABASE_ID`
2. 导入`mbti-content-creator.json`
3. 手动触发测试
4. 审核生成的内容
5. 启用定时任务

---

## 已知问题

### v1.0.0

1. **AI生成内容质量不稳定**
   - 影响：部分生成的标题或正文不够吸引人
   - 临时方案：人工审核时修改优化
   - 计划修复：v1.1.0优化AI Prompt

2. **Notion写入偶尔失败**
   - 影响：少数情况下内容未写入创作库
   - 临时方案：查看执行日志，手动重试
   - 计划修复：v1.0.1增加重试机制

3. **产品植入有时过于生硬**
   - 影响：内容可能显得像硬广
   - 临时方案：人工审核时调整文案
   - 计划修复：v1.1.0优化植入策略

---

## 路线图

### v1.1.0（预计2周后）
- 优化AI Prompt，提升内容质量
- 增加内容去重检查
- 支持多模板选择
- 增加A/B测试功能

### v1.2.0（预计1个月后）
- 新增`mbti-material-enhancer.json`
- 自动优化素材库内容
- 补充创作角度和详细信息

### v2.0.0（预计2个月后）
- 新增`mbti-content-publisher.json`
- 对接小红书API
- 对接公众号API
- 实现自动发布

### v3.0.0（预计3个月后）
- 新增`mbti-data-monitor.json`
- 自动抓取数据
- 生成分析报告
- 识别高表现内容

---

## 反馈

如有问题或建议，请通过以下方式反馈：

1. 查看执行日志
2. 查看COLLABORATION_GUIDE.md
3. 联系技术支持

---

**最后更新**：2025-12-31  
**维护者**：Manus AI + Claude Code
