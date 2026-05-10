# RimMind Organization

> 为 RimWorld 注入 AI 灵魂 —— 基于 LLM 的动态叙事与智能决策模组套件

## 关于 RimMind

RimMind 是一套为 [RimWorld](https://rimworldgame.com/) 开发的 AI Mod 套件，通过大语言模型（LLM）为游戏中的殖民者（Pawn）赋予动态人格、记忆、对话和智能决策能力。

## 核心模组

| 模组 | 说明 | 状态 |
|------|------|------|
| **RimMind-Core** | AI 客户端基础设施，异步请求队列，Prompt 组装系统 | 核心依赖 |
| **RimMind-Actions** | 意图→动作执行库，25+ 内置游戏动作 | 功能模块 |
| **RimMind-Advisor** | AI 顾问系统，为小人智能选择最优行动 | 功能模块 |
| **RimMind-Personality** | 人格系统，每日 LLM 评估生成动态 Thought | 功能模块 |
| **RimMind-Dialogue** | 对话系统，事件拦截与玩家主动对话 | 功能模块 |
| **RimMind-Memory** | 三层记忆系统（Active/Archive/Dark） | 功能模块 |
| **RimMind-Storyteller** | AI 叙事者，动态事件链与张力系统 | 功能模块 |
| **Bridge-RimTalk** | 与 RimTalk 模组的协调层 | 兼容桥接 |
| **Bridge-RimChat** | 与 RimChat 模组的协调层 | 兼容桥接 |

## 技术栈

- **C# / .NET Framework 4.8** — 模组核心
- **Harmony 2.x** — 运行时方法补丁
- **LLM API** — OpenAI 兼容接口（支持 DeepSeek / Qwen 等）
- **RimWorld 1.6** — 目标游戏版本

## 参与贡献

我们欢迎所有形式的贡献！请阅读 [贡献指南](../CONTRIBUTING.md) 了解如何参与。

## 社区

- [Issues](https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Core/issues) — 报告问题或请求功能
- [Discussions](https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Core/discussions) — 交流想法与分享经验

---

*RimMind 是一个社区驱动的开源项目，与 Ludeon Studios 无官方关联。*
