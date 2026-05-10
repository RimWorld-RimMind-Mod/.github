# 贡献指南

感谢你对 RimMind 项目的兴趣！本指南将帮助你顺利参与贡献。

## 如何贡献

### 报告问题

在提交 Issue 之前，请先搜索现有 Issue 列表，确认问题未被报告过。

提交 Bug 报告时，请包含以下信息：
- RimWorld 版本和 RimMind 版本
- 问题描述和复现步骤
- 预期行为与实际行为
- 相关日志文件（`Player.log`）
- 截图（如适用）

### 请求功能

功能请求请使用 Issue 模板，并说明：
- 功能的用途和目标
- 建议的实现方案（如有）
- 是否愿意自行实现

### 提交代码

1. **Fork 仓库** 并创建功能分支
2. **遵循开发规范**（见下方）
3. **编写测试**（如适用）
4. **提交 Pull Request** 并使用提供的 PR 模板

## 开发规范

### 代码风格

- 语言：C# + XML
- 遵循项目现有代码风格
- Harmony PostFix 优先于 Prefix/Transpiler
- 所有字符串 UI 文本通过 Keyed/XML 本地化，禁止硬编码中文
- 日志前缀格式：`[RimMind-XXX]`
- Harmony ID 格式：`mcocdaa.RimMindXXX`

### 技术约束

- **线程安全**：所有 AI 调用必须异步，结果通过 `ConcurrentQueue` 传回主线程。严禁在后台线程调用任何 RimWorld/Unity API。
- **任务打断**：使用 `TryTakeOrderedJob` 打断小人任务，禁止仅用 `StartJob`
- **性能**：禁止在 Tick 方法中直接遍历地图所有小人，使用 `IsHashIntervalTick(N)` 节流高频检查
- **存档兼容**：AI 生成的数据通过 `ExposeData` / `PostExposeData` 序列化，正确区分 `GameComponent` 与 `WorldComponent`

### 提交信息格式

遵循 Conventional Commits：

```
<type>: <description>

<optional body>
```

类型：
- `feat` — 新功能
- `fix` — 修复
- `refactor` — 重构
- `docs` — 文档
- `test` — 测试
- `chore` — 构建/工具
- `perf` — 性能优化
- `ci` — CI/CD

## 开发环境

### 前置要求

- Visual Studio 2022 或 VS Code + C# Dev Kit
- .NET Framework 4.8 开发者包
- RimWorld 1.6
- 设置 `RIMWORLD_DIR` 环境变量指向 RimWorld 安装目录

### 构建

```bash
# 构建整个解决方案
dotnet build

# 构建特定模组
dotnet build RimMind-Core/RimMind-Core.csproj
```

构建成功后，DLL 将自动通过 `robocopy` 部署到 RimWorld 的 Mods 目录。

## 模组开发指南

### 添加新子模组

1. 创建新的类库项目，目标框架 `net48`
2. 添加对 `RimMind-Core` 的引用
3. 在 `RimMindAPI` 注册上下文提供器（如需要）
4. 遵循现有模组的目录结构和命名约定

### 核心 API 使用

```csharp
// 注册上下文提供器
RimMindAPI.RegisterPawnContextProvider("MyMod", pawn => new PromptSection(...));

// 发送 AI 请求
RimMindAPI.EnqueueRequest(new AIRequest {
    Prompt = prompt,
    OnComplete = response => { /* 在主线程执行 */ }
});
```

## 社区

- 有疑问？在 [Discussions](https://github.com/RimWorld-RimMind-Mod/RimWorld-RimMind-Mod-Core/discussions) 发起讨论

## 许可

通过提交代码，你同意你的贡献将采用与项目相同的许可证（MIT）。
