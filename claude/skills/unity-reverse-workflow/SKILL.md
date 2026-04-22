---
name: unity-reverse-workflow
description: 通用 Unity 逆向协作工作流。只要用户提到以下任一类需求，就应主动触发本 skill：Unity 逆向、游戏逆向、资源逆向、sharedassets、resources.assets、AssetBundle、bundle、global-metadata.dat、Managed 程序集、TextAsset、Texture2D、Sprite、SpriteAtlas、AudioClip、提取图片、提取立绘、提取图标、提取音效、提取 BGM、提取语音、提取关卡、提取剧情文本、提取配置表、分析关卡数据、分析资源结构、盘点 Unity 资源。即使用户没有明确说“用 skill”或没有点名 UnityPy，只要问题明显属于 Unity 项目资源提取、资源分层、关卡排查或 metadata/Managed 入口分析，也要优先使用本 skill。
---

# Unity Reverse Workflow

用于指导 Claude 在本机工具有限时，高效完成 Unity 项目的通用逆向协作工作。

本 skill 不绑定具体项目结构，重点解决三类任务：
1. 提取图片资源
2. 提取音效资源
3. 排查关卡 / 文本 / 配置 / metadata / Managed 入口

## 可按需读取的参考文档
在以下情况，主动读取对应 reference：
- 需要用 `UnityPy` 做对象枚举、抽样导出、类型分布统计时：`references/unitypy-examples.md`
- 重点目标是提取 `AudioClip` / BGM / SFX / 语音时：`references/audio-extraction.md`
- 重点目标是寻找关卡、文本、配置、桥接键时：`references/level-hunting.md`
- 需要判断工具不足时如何引导安装、如何给降级路径时：`references/tool-installation.md`

## 当前默认优先工具
优先使用当前机器已确认存在的工具：
- `UnityPy`
- `python3`
- `dotnet`
- `strings`
- `xxd`
- `hexdump`
- `pip3`

如果用户机器上没有这些工具，不要直接卡住。先读取 `references/tool-installation.md`，按“先盘点已有工具、再按需安装”的原则推进。

## 核心方法
把 Unity 逆向对象拆成五层：
1. 容器层
2. 对象层
3. 语义层
4. 引用层
5. 闭环层

默认原则：
- 先分层，再提取
- 先样本，再批量
- 先对象层，再语义层
- 先 bridge/key，再做闭环宣称

## 标准工作流
### 阶段 1：快速盘点
先回答：
1. 项目里有哪些 Unity 关键容器？
2. 当前任务更像图片 / 音效 / 关卡 / metadata 哪一类？
3. 当前机器已有工具能直接推进到哪一步？

### 阶段 2：按目标进入专项路径
- 图片：优先 `Texture2D` / `Sprite` / `SpriteAtlas`
- 音效：优先 `AudioClip`
- 关卡 / 文本 / 配置：优先 `TextAsset` / `MonoBehaviour` / `.bytes` / `json`
- metadata / Managed：优先类名、字段名、系统词入口

### 阶段 3：输出结果
每次输出至少包含：
1. 当前目标
2. 已确认事实
3. 暂不能下结论的点
4. 下一步最合理动作

## 不要做的事
- 不要先假定具体项目结构
- 不要无脑全量导出
- 不要把展示层、系统层或文本近似直接升格为关卡闭环
- 不要在没有对象级入口时声称已经定位到关卡数据

## 何时切线
如果连续两轮都只能新增：
- 更多字符串命中
- 更多展示近邻
- 更多文本块

但没有新增对象级 bridge/key，就应主动建议切线到：
- Managed / metadata
- 更底层二进制/序列化层
- 单个强样本 key 追踪
