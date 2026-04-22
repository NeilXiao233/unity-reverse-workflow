# Unity Reverse Workflow

跨平台、面向多种 agent 的 Unity 逆向协作 skill / workflow 包。

目标：让 Claude Code、Codex、OpenCode 等 agent 在遇到 Unity 逆向、资源提取、关卡排查、metadata/Managed 分析任务时，都能遵循同一套分层方法和输出规范推进工作。

## 适用场景
当用户提到以下任务时，应优先使用本 workflow：
- Unity 逆向 / 游戏逆向 / 资源逆向
- 提取图片 / 立绘 / 图标 / Sprite
- 提取音效 / BGM / 语音 / AudioClip
- 提取关卡 / 剧情文本 / 配置表 / TextAsset
- 分析 sharedassets / resources.assets / AssetBundle / bundle
- 分析 `global-metadata.dat` / Managed 程序集
- 想先做一轮 Unity 资源盘点，再决定深入方向

## 仓库结构

```text
unity-reverse-workflow/
├── README.md
├── shared/
│   ├── references/
│   │   ├── unitypy-examples.md
│   │   ├── audio-extraction.md
│   │   ├── level-hunting.md
│   │   └── tool-installation.md
│   └── evals/
│       └── evals.json
├── claude/
│   └── skills/
│       └── unity-reverse-workflow/
│           └── SKILL.md
├── codex/
│   └── skills/
│       └── unity-reverse-workflow.md
└── opencode/
    └── skills/
        └── unity-reverse-workflow.md
```

## 平台使用方式

### Claude Code
把 `claude/skills/unity-reverse-workflow/` 目录复制到目标项目的：
- `.claude/skills/`

最终路径示例：
- `.claude/skills/unity-reverse-workflow/SKILL.md`

并把 `shared/references/` 的文档复制到该 skill 目录下的 `references/`。

### Codex
把 `codex/skills/unity-reverse-workflow.md` 作为 Codex 可读的 skill / prompt 文档使用。

推荐做法：
- 放入你的 Codex 自定义 skills/prompts 目录
- 或直接在会话启动时把它作为工作流说明加载

同时把 `shared/references/` 一并保留，供 agent 按需读取。

### OpenCode
把 `opencode/skills/unity-reverse-workflow.md` 作为 OpenCode 的工作流 skill 文档使用。

同样建议：
- 保留 `shared/references/`
- 把工具安装说明和通用方法一起带上

## 工具策略
本 workflow 不要求所有使用者预先装好全套工具。

默认策略：
1. **先盘点已有工具**
2. **先用已有工具做最小成本验证**
3. **只有在当前目标确实需要时，才引导安装额外工具**

这意味着：
- 不应一开始就要求用户安装大而全工具链
- 应先说明“现在已有工具能做什么”
- 再说明“若想继续到下一层，需要补什么工具”

详情见：
- `shared/references/tool-installation.md`

## 当前默认优先工具
- `UnityPy`
- `python3`
- `dotnet`
- `strings`
- `xxd`
- `hexdump`
- `pip3`

## 工作原则
- 先分层，再提取
- 先样本，再批量
- 先对象层，再语义层
- 先 bridge/key，再做闭环宣称
- 找不到对象级入口时，要主动切线

## 当前包含的参考文档
- `shared/references/unitypy-examples.md`
- `shared/references/audio-extraction.md`
- `shared/references/level-hunting.md`
- `shared/references/tool-installation.md`

## 测试 prompts
已提供一组通用 eval prompts：
- `shared/evals/evals.json`

它们覆盖：
- Unity 资源分层盘点
- 图片提取
- 音频提取
- 关卡 / 文本 / 配置排查
- Managed / metadata 桥接搜索

## 本地 git 使用
当前版本默认先做“本地 git 仓库”。

初始化后可在仓库目录执行：
- `git status`
- `git add ...`
- `git commit ...`

后续如果你要推远端，再补 remote 即可。
