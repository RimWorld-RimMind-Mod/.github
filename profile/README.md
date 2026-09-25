# RimWorld-RimMind-Mod

> **为 RimWorld 注入 AI 灵魂** —— 基于大语言模型（LLM）的多智能体动态叙事、人格塑造与自主决策模组套件。

[![RimWorld Version](https://img.shields.io/badge/RimWorld-1.6-brightgreen.svg)](https://rimworldgame.com/)
[![Harmony](https://img.shields.io/badge/Harmony-2.x-blue.svg)](https://github.com/pardeike/Harmony)
[![.NET](https://img.shields.io/badge/.NET%20Framework-4.8-purple.svg)](https://dotnet.microsoft.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 📖 关于 RimMind

**RimMind** 是一套面向 **RimWorld 1.6** 的现代化模块化 AI 模组生态系统。通过将现代大语言模型（LLM）的多智能体协同技术与游戏原生机制深度结合，为边缘世界中的殖民者（Pawn）赋予深度的心理活动、长期记忆、自然语言对话和自主意图决策能力。

套件由 **1 个核心基础设施底座（Core）**、**6 大功能玩法模组（Actions, Advisor, Dialogue, Personality, Memory, Storyteller）**、**1 个扩展模型网关模组（Extension-ModelService）** 与 **2 个第三方兼容桥（Bridge-RimTalk, Bridge-RimChat）** 共 **10 个独立可加载模组** 组成。每个模组均可独立发布与选用，遵循严格的单向依赖与单一职责原则。

---

## 🌟 五大核心架构创新

1. **⚡ 4-Zone Prompt Caching (KV-Cache) 拓扑**
   - 针对现代大模型前缀缓存机制特化设计：
     - **Zone 1 (Static Instructions)**：系统全局指令与 Tool 规范，100% 字节不变。
     - **Zone 2 (Pawn Profiles)**：殖民者稳定人设、特质与背景故事。
     - **Zone 3 (Histories & Memories)**：近期对话历史与时序记忆，单调追加。
     - **Zone 4 (Volatile Observations Tail)**：游戏时间、动态天气、即时心情等易变环境严格隔离在尾部。
   - 彻底消除易变时间戳对前置缓存的击穿，生产环境预估**缓存命中率达 80%~90%**，大幅降低 Token 消耗并加速推理。

2. **💬 全面 ToolCall 化与 `express_dialogue` 契约**
   - 彻底告别易产生幻觉与解析错误的非结构化自由 JSON 回复。
   - 对话交互与心理/关系更新统一走 `express_dialogue` 结构化工具调用。
   - 具备独白安全归零截断、relation_delta 范围钳位与双模式解析（Nested Function 与 Flat Arguments）多级回退策略。

3. **🎲 基于概率的状态跃迁触发体系**
   - 废弃机械死板的定时间隔轮询，引入基于生活事件的概率跃迁（如晨光破晓 6:00、观星/娱乐触发、心情剧烈波动）。
   - 独立计算触发概率并搭配动态活动频率缩放（ActivityFrequencyScale），赋予小人真实、生动的自主生活节奏。

4. **🖥️ F8 现代化实机调试中心 (RimMind Hub) 与殖民者观察台**
   - 游戏内按快捷键一键呼出，集成实时 LLM 连通性测速探针、全局队列与冷却重置控制台、运行时参数微调。
   - 殖民者实时认知观察器与全链路报文检查器（ContextPayloadInspector），毫秒级观测 4-Zone 上下文。
   - 智能收缩请求悬浮窗（RequestOverlay），审批完成后自动平滑收缩为迷你胶囊 `[Pending: 0]`。

5. **🔌 动态模型扩展与高可用负载均衡 (`RimMindAPI.Ext`)**
   - Core 保持纯粹公共抽象，不与任何具体商业服务商硬编码耦合。
   - Extension-ModelService 动态注入 OpenCode Go 订阅直连、Anthropic 原生协议与 OpenAI 兼容端点。
   - 提供主备故障转移（Priority Failover）、轮询与 60s 熔断保护，本地回环地址安全隔离。

---

## 🏛️ 系统架构与模块全景

```mermaid
flowchart TD
    subgraph External["外部前置与兼容目标"]
        Harmony["Harmony (brrainz.harmony)"]
        RimWorld["RimWorld 1.6 (Verse / RimWorld)"]
        RimTalkMod["RimTalk (可选第三方)"]
        RimChatMod["RimChat (可选第三方)"]
    end

    subgraph CoreLayer["核心底座 (Core Foundation)"]
        Core["RimMind-Core\n(4-Zone KV Cache / 异步队列 / AgentBus / ToolCall 网关)"]
    end

    subgraph FeatureLayer["玩法与智能子模组 (Gameplay & Agents)"]
        Actions["RimMind-Actions\n(复合工具 / 机制动作)"]
        Advisor["RimMind-Advisor\n(决策循环 / 建议审批)"]
        Dialogue["RimMind-Dialogue\n(express_dialogue / 社交关系)"]
        Personality["RimMind-Personality\n(概率状态跃迁 / 心理 Thought)"]
        Memory["RimMind-Memory\n(三层记忆 / 时间上下文)"]
        Storyteller["RimMind-Storyteller\n(AI 叙事者 / 事件张力)"]
    end

    subgraph ExtensionLayer["扩展模型网关层 (Model Providers)"]
        ModelService["RimMind-Extension-ModelService\n(OpenCode Go / Anthropic / 容灾负载均衡)"]
    end

    subgraph BridgeLayer["兼容桥接层 (Bridges)"]
        BridgeTalk["Bridge-RimTalk"]
        BridgeChat["Bridge-RimChat"]
    end

    %% 核心依赖
    Core --> Harmony
    Core --> RimWorld
    Actions -. 编译依赖 .-> Core
    Advisor -. 编译依赖 .-> Core
    Dialogue -. 编译依赖 .-> Core
    Personality -. 编译依赖 .-> Core
    Memory -. 编译依赖 .-> Core
    Storyteller -. 编译依赖 .-> Core
    BridgeTalk -. 编译依赖 .-> Core
    BridgeChat -. 编译依赖 .-> Core
    ModelService -. 编译依赖 .-> Core

    %% 模组加载与协作
    Advisor -->|LoadAfter / modDependency| Actions
    Storyteller -->|LoadAfter / modDependency| Memory
    BridgeTalk --> RimTalkMod
    BridgeChat --> RimChatMod

    %% 运行时数据流解耦
    Personality ==>|Thought & Context| Advisor
    Dialogue ==>|Thought & Context| Advisor
    Memory ==>|Context Provider| Core
    Actions ==>|注册高级 ToolCall| Core
    Advisor ==>|RimMindAPI.Tools 调度| Core
    ModelService ==>|RimMindAPI.Ext 动态注入| Core
```

---

## 📦 开源模组矩阵与仓库索引

所有模组均独立维护与发布，玩家可按需搭配使用（**RimMind-Core 为所有子模组的唯一共同前置**）：

### 核心底座 (Core)
| 仓库 | PackageId | 说明 |
|---|---|---|
| [**RimWorld-RimMind-Mod-Core**](https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Core) | `mcocdaa.RimMindCore` | 统一 4-Zone LLM 请求管线、异步优先队列、上下文引擎、AgentBus、Tool 注册网关与公共 API |

### 玩法与决策模组 (Gameplay)
| 仓库 | PackageId | 说明 |
|---|---|---|
| [**RimWorld-RimMind-Mod-Advisor**](https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Advisor) | `mcocdaa.RimMindAdvisor` | AI 顾问决策循环：小人自主意图分析、玩家审批流与 Tool Calling 执行反馈闭环 |
| [**RimWorld-RimMind-Mod-Actions**](https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Actions) | `mcocdaa.RimMindActions` | 复合工具（Composite ToolCall）编排与 25+ 游戏机制高阶动作库 |
| [**RimWorld-RimMind-Mod-Personality**](https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Personality) | `mcocdaa.RimMindPersonality` | 概率状态跃迁驱动的殖民者动态心理状态评估、人格特质演变与原生 Thought 注入 |
| [**RimWorld-RimMind-Mod-Dialogue**](https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Dialogue) | `mcocdaa.RimMindDialogue` | 基于 `express_dialogue` 的殖民者社交/事件拦截对话、玩家主动对话与好感度演化 |
| [**RimWorld-RimMind-Mod-Memory**](https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Memory) | `mcocdaa.RimMindMemory` | 三层记忆架构（活动/归档/潜意识暗记忆）与时序上下文持久化 |
| [**RimWorld-RimMind-Mod-Storyteller**](https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Storyteller) | `mcocdaa.RimMindStoryteller` | 基于世界张力与殖民地历史叙事线的 AI 动态事件叙事者 |

### 扩展模型服务 (Extension)
| 仓库 | PackageId | 说明 |
|---|---|---|
| [**RimWorld-RimMind-Mod-Extension-ModelService**](https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Extension-ModelService) | `mcocdaa.RimMindExtensionModelService` | 多端点模型服务扩展：OpenCode Go 订阅直连、Anthropic 缓存协议适配与高可用故障转移负载均衡 |

### 第三方兼容桥 (Bridges)
| 仓库 | PackageId | 说明 |
|---|---|---|
| [**RimWorld-RimMind-Mod-Bridge-RimTalk**](https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Bridge-RimTalk) | `mcocdaa.RimMindBridgeRimTalk` | 与 RimTalk 模组的对话门控协调、人设数据互通与上下文推送桥 |
| [**RimWorld-RimMind-Mod-Bridge-RimChat**](https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Bridge-RimChat) | `mcocdaa.RimMindBridgeRimChat` | 与 RimChat 模组的对话/动作互斥与上下文拉取桥 |

### 共享基础设施
| 仓库 | 说明 |
|---|---|
| [**Workflows**](https://github.com/RimWorld-RimMind-Mod/Workflows) | 跨仓库复用的 GitHub Actions 工作流（CI 测试矩阵与 AI 双语 Release Notes） |
| [**.github**](https://github.com/RimWorld-RimMind-Mod/.github) | 组织级配置、社区指引与 Issue/PR 模板 |

---

## 🔗 三维依赖设计规范

为了保证系统的健壮性与模组独立性，RimMind 严格区隔三个层面的依赖关系：

1. **C# 编译期程序集依赖（硬依赖）**：
   - 所有子模组均**仅引用 Core 输出的三个纯净程序集**（`0_RimMindDomain.dll`, `1_RimMindApplication.dll`, `2_RimMindCore.dll`）。
   - 子模组之间**绝对禁止**产生直接的 C# 项目工程引用或 DLL 互引。
2. **RimWorld 游戏加载顺序（`About.xml`）**：
   - 基础排序：`Harmony` → `RimMind - Core` → `各玩法子模组 (Actions/Memory/Personality/Dialogue)` → `Advisor` → `Storyteller` → `Bridge-RimTalk / Bridge-RimChat`。
3. **运行时数据流解耦**：
   - **Thought 状态通道**：Personality 与 Dialogue 将状态写入游戏原生 Thought，Advisor 仅通过 Core 提供的 Context Provider 异步感知，不直连对应模组类。
   - **ToolCall 抽象**：Advisor 仅调用 Core 的 `RimMindAPI.Tools`，具体动作由 Actions 注册。
   - **AgentBus 消息总线**：异步计算派发感知事件，主线程 tick 时安全消费并执行 Verse/Unity 副作用。

---

## 🚀 玩家与开发者指南

### 🎮 玩家安装与模组启用
1. 确保安装前置模组 **Harmony**。
2. 必须启用 **RimMind - Core**（核心底座）。
3. 按照个人喜好自由启用玩法子模组（如 Advisor 顾问、Dialogue 对话、Personality 人格等）。
4. 若同时安装了第三方对话模组（RimTalk / RimChat），请启用对应的 Bridge 模组以获得最佳协调体验。

### 🛠️ 开发者与贡献者
各模组作为独立 Git 仓库管理。贡献代码时，可直接克隆对应的模组仓库：

```bash
# 克隆核心仓库
git clone https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Core.git

# 或克隆指定玩法模组
git clone https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Advisor.git
```

编译测试要求：.NET 10 SDK（编译测试工程）与 .NET Framework 4.8（RimWorld 运行环境）。

```powershell
# 编译并运行模组测试
dotnet test Tests/RimMindCore.Tests.csproj -c Release
```

---

## 🤝 参与贡献

我们欢迎社区贡献！
- 提交 Bug 或需求：请前往对应模组的 [Issues](https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Core/issues)。
- 了解开发规范：请查阅各仓库根目录的 `AGENTS.md` 与 [CONTRIBUTING.md](CONTRIBUTING.md)。

---

*RimMind 是一个社区驱动的开源模组套件，遵循 MIT 开源协议，与 Ludeon Studios 无官方关联。*
