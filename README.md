# 从数据到生产：AI 模型的完整生命周期

> From Data to Production: The Complete Lifecycle of AI Models

一次技术分享，梳理 AI 模型从原始数据到线上生产环境所经历的完整旅程。

## 大纲

1. **数据准备** — 数据采集、清洗、标注与数据集划分
2. **模型训练** — 训练框架、超参数调优、实验跟踪
3. **评估与验证** — 离线指标、偏差分析、模型选择
4. **部署上线** — 模型压缩、服务化、推理优化
5. **监控与迭代** — 线上监控、数据漂移、持续再训练

## 分享材料

- [`docs/superpowers/specs/2026-09-27-ai-model-lifecycle-talk-design.md`](docs/superpowers/specs/2026-09-27-ai-model-lifecycle-talk-design.md) — 分享设计文档（结构、时间分配、讲稿要点、事实出处）
- [`docs/talk/slides-content.md`](docs/talk/slides-content.md) — 页面级 PPT 内容（50 页：标题/要点/讲稿备注/配图建议）
- [`docs/research-verified-facts.md`](docs/research-verified-facts.md) — 人工核验事实底稿（A-F 六节，置信度分级：3 票验证 / 逐字核对 / 单源引文）
- [`docs/research/deep-research-digest.md`](docs/research/deep-research-digest.md) — 调研事实库（25 来源 / 125 论断 / 13 条经对抗验证）

## 目录结构

```
docs/    # 分享材料与文档
slides/  # HTML 演示文稿（open slides/index.html，S 演讲者视图）
demo/    # 演示代码（建设中）
```

## 工具链

- **[html-ppt](https://github.com/lewislulu/html-ppt-skill) skill** — 制作 HTML 演示文稿（36 主题 / 36 布局 / 15 全套模板 / 演讲者模式），装在 `.claude/skills/html-ppt/`（已 gitignore，clone 后本地安装一次）：

  ```bash
  npx skills add https://github.com/lewislulu/html-ppt-skill -a claude-code -y
  ```

  安装后当前会话运行 `/reload-skills` 即可生效；用 `/html-ppt` 或直接说"做一份 PPT"调用。
