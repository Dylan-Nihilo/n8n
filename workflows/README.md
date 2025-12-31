# MBTI茶饮营销自动化工作流

## 📖 项目简介

这是一套完整的MBTI主题茶饮品牌营销自动化工作流系统，基于n8n构建，实现从素材采集、内容创作到数据监测的全流程自动化。

**品牌**：原野雾芽  
**理念**：不被定义，自己定义  
**产品**：4款MBTI主题茶饮（金桂乌龙、冷萃白桃、茉莉绿茶、红玉红茶）

---

## 🎯 工作流清单

### 1. mbti-rss-collector.json (V1)
**功能**：从知乎热榜RSS采集MBTI相关素材

**特点**：
- 数据源：知乎热榜RSS
- 过滤：23个MBTI关键词
- 执行：每12小时
- 节点：8个

**状态**：✅ 已部署

---

### 2. mbti-rss-collector-v2.json (V2)
**功能**：主动搜索并智能分析MBTI话题

**特点**：
- 数据源：SerpAPI主动搜索
- 内容：抓取完整网页正文
- 分析：AI深度分析（Claude Sonnet 4.5）
- 过滤：契合度≥6.0
- 执行：每天9:00
- 节点：11个

**状态**：✅ 已部署

**工作流程**：
```
触发器 → 搜索MBTI话题 → 提取结果 → 抓取网页 → 
提取HTML → AI分析 → 整合数据 → 构建请求 → 
写入Notion → 汇总 → 生成摘要
```

---

### 3. mbti-content-creator.json (NEW)
**功能**：基于素材库和产品库自动生成小红书和公众号内容

**特点**：
- 输入：素材库（热度≥8） + 产品库
- 输出：小红书笔记 + 公众号文章
- AI生成：标题 + 正文 + 标签
- 执行：周一、三、五 9:00
- 节点：17个

**状态**：🆕 新增

**工作流程**：
```
触发器 → 查询素材库 → 查询产品库 → 选择数据 →
分支1: AI生成小红书标题 → AI生成小红书正文 → 整合内容 → 写入创作库
分支2: AI生成公众号标题 → AI生成公众号正文 → 整合内容 → 写入创作库
→ 汇总结果 → 生成摘要
```

**输出示例**：
- 小红书：800-1000字图文笔记 + 标签 + 配图建议
- 公众号：2000-3000字深度文章

---

## 🗂️ Notion数据库结构

### 素材库 (Material Database)

| 字段名 | 类型 | 说明 |
|-------|------|------|
| 标题 | Title | 素材标题 |
| 来源 | Select | 微博/知乎/小红书等 |
| 原文链接 | URL | 原文地址 |
| 原文摘要 | Rich Text | 100-500字摘要 |
| 详细内容 | Rich Text | 完整内容（可选） |
| 创作角度 | Rich Text | AI生成的5个创作方向 |
| 热度评分 | Number | 1-10分 |
| 契合度 | Number | 1-10分 |
| 话题分类 | Multi-select | MBTI/热点/社交等 |
| MBTI相关 | Multi-select | MBTI类型标签 |
| 状态 | Select | 待评估/可用/已使用 |
| 使用次数 | Number | 被使用的次数 |

### 产品库 (Product Database)

| 字段名 | 类型 | 说明 |
|-------|------|------|
| 产品名称 | Title | 茶饮名称 |
| MBTI类型 | Select | INFP/INTJ/ENFP/ESTJ |
| 人格特质 | Rich Text | 性格描述 |
| 核心卖点 | Rich Text | 主要特点 |
| 适合场景 | Rich Text | 使用场景 |
| 口味描述 | Rich Text | 味道特点 |
| 价格 | Number | 售价 |
| 状态 | Select | 在售/下架 |

### 创作库 (Creation Database)

| 字段名 | 类型 | 说明 |
|-------|------|------|
| 标题 | Title | 内容标题 |
| 平台 | Select | 小红书/公众号 |
| 内容类型 | Select | 图文笔记/深度文章 |
| 正文 | Rich Text | 完整内容 |
| 标签 | Multi-select | 话题标签（小红书） |
| 配图建议 | Rich Text | 图片说明 |
| 素材来源 | Rich Text | 关联的素材 |
| 关联产品 | Rich Text | 推广的产品 |
| 状态 | Select | 待审核/已审核/已发布 |
| 创建时间 | Date | 创建日期 |
| 发布时间 | Date | 发布日期 |
| 阅读量 | Number | 阅读数 |
| 点赞数 | Number | 点赞数 |
| 评论数 | Number | 评论数 |

---

## ⚙️ 环境变量配置

在n8n中配置以下环境变量：

```bash
# Notion配置
NOTION_API_TOKEN=secret_xxx                          # Notion API密钥
NOTION_MATERIAL_DATABASE_ID=2d87fa67a9af81b99b07cde5013942b4  # 素材库ID
NOTION_PRODUCT_DATABASE_ID=xxx                       # 产品库ID
NOTION_CREATION_DATABASE_ID=xxx                      # 创作库ID

# AI服务配置
AI_API_BASE_URL=http://localhost:8045                # AI API地址
AI_MODEL=gpt-4o-mini                                 # AI模型（可选）
OPENAI_API_KEY=sk-xxx                                # OpenAI API密钥

# 搜索服务配置
SERPAPI_KEY=xxx                                      # SerpAPI密钥

# 社交媒体配置（未来使用）
XIAOHONGSHU_API_KEY=xxx                              # 小红书API密钥
WECHAT_APPID=xxx                                     # 微信公众号AppID
WECHAT_APPSECRET=xxx                                 # 微信公众号AppSecret
```

### 获取Notion Database ID

1. 打开Notion数据库页面
2. 点击右上角"···" → "Copy link"
3. 链接格式：`https://www.notion.so/xxx?v=yyy`
4. `xxx`部分就是Database ID（去掉中间的`-`）

---

## 🚀 快速开始

### 1. 准备Notion数据库

创建3个数据库，按照上述结构添加字段：
- 素材库
- 产品库
- 创作库

### 2. 配置环境变量

在n8n的Settings → Environment Variables中添加所有必需的环境变量。

### 3. 导入工作流

1. 在n8n中点击"Import from File"
2. 选择工作流JSON文件
3. 依次导入：
   - mbti-rss-collector-v2.json
   - mbti-content-creator.json

### 4. 测试运行

1. 先运行`mbti-rss-collector-v2`，采集素材
2. 在素材库中将状态改为"可用"
3. 在产品库中添加4款产品
4. 运行`mbti-content-creator`，生成内容
5. 在创作库中审核内容

### 5. 启用定时任务

在每个工作流中启用"Active"开关，工作流将按计划自动执行。

---

## 📅 执行时间表

| 工作流 | 执行时间 | 频率 | 说明 |
|-------|---------|------|------|
| mbti-rss-collector-v2 | 每天9:00 | 每天 | 素材采集 |
| mbti-content-creator | 周一三五 9:00 | 每周3次 | 内容创作 |

**建议时间线（周一）**：
```
09:00 - 素材采集 → 新增5条素材
09:30 - 内容创作 → 生成2篇内容
10:00 - 人工审核 → 修改优化
11:00 - 手动发布 → 发布到平台
```

---

## 🎨 内容创作策略

### 小红书内容

**结构**：
- 开头引入：100字，吸引眼球
- MBTI知识：400字，提供价值
- 产品植入：300字，自然过渡
- 结尾互动：100字，引导评论

**风格**：
- 轻松活泼
- 使用emoji
- 真实可信
- 避免硬广

**禁忌词**：绝对、必买、最好、第一、保证、神器

### 公众号内容

**结构**：
- 引言：200字，提出问题
- 4-5个小节：各400字，深度分析
- 产品介绍：300字，自然植入
- 结尾：200字，情感共鸣

**风格**：
- 深度有料
- 情感共鸣
- 提供洞察
- 品牌理念

---

## 📊 预期效果

### 第1个月
- 素材库：150条
- 创作库：24篇（12小红书 + 12公众号）
- 发布量：20篇（审核通过率80%+）

### 第3个月
- 素材库：450条
- 创作库：72篇
- 发布量：60篇

### 第6个月
- 素材库：900条
- 创作库：144篇
- 发布量：120篇
- 粉丝：3000+

---

## 🔧 故障排查

### 问题1：AI生成失败

**可能原因**：
- AI API地址错误
- API密钥无效
- 模型名称错误
- 网络超时

**解决方案**：
1. 检查`AI_API_BASE_URL`是否正确
2. 确认`OPENAI_API_KEY`有效
3. 尝试更换模型（gpt-4o-mini）
4. 增加超时时间（60秒）

### 问题2：Notion写入失败

**可能原因**：
- Database ID错误
- API Token无效
- 字段名称不匹配
- 数据格式错误

**解决方案**：
1. 确认Database ID正确（32位，无`-`）
2. 检查Notion API Token权限
3. 确保数据库字段名称完全一致
4. 查看n8n执行日志的详细错误

### 问题3：素材库为空

**可能原因**：
- 搜索结果为空
- 过滤条件太严格
- 网页抓取失败

**解决方案**：
1. 手动运行`mbti-rss-collector-v2`
2. 降低契合度阈值（从6.0降到5.0）
3. 检查SerpAPI密钥和额度
4. 查看执行日志

---

## 📚 相关文档

- [COLLABORATION_GUIDE.md](./COLLABORATION_GUIDE.md) - Manus与Claude Code协同指南
- [CHANGELOG.md](./CHANGELOG.md) - 版本变更记录
- [optimization_plan.md](./optimization_plan.md) - 优化方案详解
- [workflow_analysis.md](./workflow_analysis.md) - 工作流分析

---

## 🤝 贡献

本项目由Manus AI设计，与Claude Code协同开发。

**工作流设计**：Manus  
**本地部署**：Claude Code  
**内容审核**：人工  

---

## 📞 支持

如有问题，请：
1. 查看故障排查部分
2. 查看n8n执行日志
3. 查看COLLABORATION_GUIDE.md
4. 联系技术支持

---

## 📝 许可

本项目仅供学习和个人使用。

---

**最后更新**：2025-12-31  
**版本**：v1.0
