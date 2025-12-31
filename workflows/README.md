# 营销自动化系统 v2.0

> 模块化、通用化、可扩展的营销自动化解决方案

## 🎯 系统特点

- **模块化设计**：每个功能独立，可自由组合
- **通用化配置**：适配任何产品、任何行业
- **多平台支持**：n8n、Dify等多种工作流引擎
- **全流程覆盖**：热点抓取 → 内容创作 → 发布 → 监测 → 优化
- **数据驱动**：基于Notion的统一数据管理

## 📊 系统架构

```
配置层 (Config) → 数据层 (Notion) → 工作流层 (n8n/Dify) → 执行层 (Platforms)
```

详见：[ARCHITECTURE.md](ARCHITECTURE.md)

## 🚀 快速开始

### 1. 环境准备

**必需服务**：
- n8n (工作流引擎)
- Notion (数据管理)
- OpenAI API (AI生成)
- SerpAPI (搜索数据)

**可选服务**：
- Dify (AI增强)
- 各平台API (自动发布)

### 2. 配置Notion数据库

按照 `notion/SCHEMA.md` 创建7个数据库：
1. 热点库 (Hotspot DB)
2. 素材库 (Material DB)
3. 产品库 (Product DB)
4. 内容库 (Content DB)
5. 发布库 (Publish DB)
6. 数据库 (Analytics DB)
7. 策略库 (Strategy DB)

### 3. 配置环境变量

复制配置模板：
```bash
cp config/examples/mbti-tea.env .env
```

编辑 `.env` 填入你的API密钥和数据库ID。

### 4. 导入n8n工作流

在n8n中依次导入：
- `n8n/modules/01-trend-capture/trend-search.json`
- `n8n/modules/02-content-creation/content-generate-ai.json`
- `n8n/modules/03-content-publishing/publish-xiaohongshu.json`
- `n8n/modules/04-data-monitoring/monitor-metrics.json`

### 5. 测试运行

手动触发每个工作流，检查是否正常运行。

## 📦 模块说明

### 核心模块 (MVP)

| 模块 | 功能 | 文件 |
|-----|------|------|
| 热点抓取 | 搜索引擎热点 | `01-trend-capture/trend-search.json` |
| 内容创作 | AI生成内容 | `02-content-creation/content-generate-ai.json` |
| 内容发布 | 小红书发布 | `03-content-publishing/publish-xiaohongshu.json` |
| 数据监测 | 基础指标 | `04-data-monitoring/monitor-metrics.json` |

### 扩展模块 (待开发)

- RSS订阅、社交媒体监控
- 模板生成、人工审核
- 公众号、抖音、知乎发布
- 评论回复、私信处理
- A/B测试、策略优化

## 🔧 配置说明

### 通用配置

所有配置通过环境变量管理，支持：
- AI服务配置
- Notion数据库ID
- 搜索关键词
- 目标平台
- 发布策略

### 产品配置

在Notion产品库中配置：
- 产品名称、标签
- 核心卖点
- 使用场景
- 植入策略

### 策略配置

在环境变量中配置：
- 内容比例（教育/情感/促销）
- 发布时间
- 监测频率

## 📖 文档

- [系统架构](ARCHITECTURE.md) - 完整的架构设计
- [Notion结构](notion/SCHEMA.md) - 数据库设计
- [部署指南](DEPLOYMENT.md) - 详细部署步骤
- [配置指南](CONFIGURATION.md) - 配置说明
- [最佳实践](docs/best-practices.md) - 使用建议

## 🎯 使用场景

### 场景1：MBTI茶饮营销

```
热点抓取(MBTI话题) → AI生成(结合产品) → 小红书发布 → 数据监测
```

配置：`config/examples/mbti-tea.env`

### 场景2：时尚品牌营销

```
热点抓取(时尚话题) → AI生成(穿搭建议) → 多平台发布 → 数据分析
```

配置：自定义关键词和产品

### 场景3：SaaS产品营销

```
行业热点 → 解决方案内容 → 知乎/公众号 → 转化追踪
```

配置：B2B策略

## 🔄 工作流组合

### 全自动流程

```
trend-search (定时) → content-generate-ai (定时) 
→ publish-xiaohongshu (定时) → monitor-metrics (定时)
```

### 半自动流程

```
trend-search (定时) → 人工筛选 → content-generate-ai (手动)
→ 人工审核 → publish-xiaohongshu (手动) → monitor-metrics (定时)
```

## 📊 预期效果

**第1个月**：
- 自动采集150条热点
- 生成24篇内容
- 节省80%时间

**第3个月**：
- 素材库450条
- 内容库72篇
- 建立稳定节奏

**第6个月**：
- 素材库900条
- 内容库144篇
- 粉丝增长3000+

## 🤝 贡献

欢迎提交Issue和Pull Request！

## 📄 许可

MIT License

## 📞 支持

- 文档：查看 `docs/` 目录
- 问题：提交 GitHub Issue
- 讨论：GitHub Discussions

---

**版本**: v2.0  
**更新**: 2025-12-31  
**作者**: Manus AI
