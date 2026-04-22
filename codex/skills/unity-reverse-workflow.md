# unity-reverse-workflow

通用 Unity 逆向协作 workflow，适用于 Codex 场景。

## 什么时候用
当任务涉及以下任一方向时，优先使用本 workflow：
- Unity 逆向 / 游戏逆向 / 资源逆向
- 提取图片 / Sprite / Texture2D / SpriteAtlas
- 提取音效 / BGM / AudioClip
- 提取关卡 / 剧情文本 / 配置 / TextAsset
- 分析 sharedassets / resources.assets / AssetBundle / bundle
- 分析 `global-metadata.dat` / Managed 程序集

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

## 推荐流程
### 1. 快速盘点
先判断：
- 哪些文件是关键 Unity 容器
- 当前任务属于图片 / 音效 / 关卡 / metadata 哪类
- 当前机器已有工具能推进到哪一步

### 2. 按目标走分支
- 图片：优先看 `Texture2D` / `Sprite` / `SpriteAtlas`
- 音效：优先看 `AudioClip`
- 关卡 / 文本 / 配置：优先看 `TextAsset` / `MonoBehaviour` / `.bytes` / `json`
- Managed / metadata：优先找类名、字段名、系统词入口

### 3. 工具不足时的策略
不要直接卡住。
先盘点已有工具，再按需建议安装。

参考：
- `../../shared/references/tool-installation.md`
- `../../shared/references/unitypy-examples.md`
- `../../shared/references/audio-extraction.md`
- `../../shared/references/level-hunting.md`

## 输出格式
每次输出至少包含：
1. 当前目标
2. 已确认事实
3. 暂不能下结论的点
4. 下一步最合理动作

## 禁止事项
- 不要先假定项目结构
- 不要无脑全量导出
- 不要把展示层或系统层直接升格为关卡闭环
- 不要把字符串近似命中当最终绑定
