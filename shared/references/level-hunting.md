# 关卡资源排查参考

本文件给 `unity-reverse-workflow` workflow 提供通用的“关卡在哪一层”排查框架。

## 目标
帮助 agent 不再把“找到很多像关卡的东西”误当成真正闭环。

## 关卡相关资源的常见层次
### 1. 布局层
常见载体：
- `Level*.bytes`
- `stage*.bytes`
- 二进制 token 表
- 轻量配置表

这层回答的是：
- 关卡怎么排布
- 槽位结构如何

但通常还不能回答完整题面是什么。

### 2. 题面正文层
常见载体：
- `TextAsset`
- sharedassets 中连续文本块
- json/csv/dat 配置
- clue/story/data block

这层回答的是：
- subjects / traits / values / clue / narrative text

但不一定知道它属于哪个关卡编号。

### 3. 展示层
常见载体：
- prefab
- glb / hierarchy
- Sprite / Texture
- icon / portrait

这层回答的是：
- UI 长什么样
- 展示槽位长什么样

但不能直接当关卡真值层。

### 4. 系统层
常见载体：
- FTUE / onboarding / tutorial config
- story / mission / progression config
- difficulty / reward / unlock level

这层回答的是：
- 什么时候解锁系统
- 哪些系统在前期出现

但通常不是具体单关题面。

### 5. 引用桥层
这才是最关键的一层：
- level id
- template id
- document id
- object key
- runtime field bridge
- hash / header / offset 规律

没有这一层，就不要把“像关卡”写成“已经定位到关卡”。

## 标准排查顺序
### 第一步：先识别当前命中的到底是哪一层
对每个新发现，先判断它属于：
- 布局层
- 正文层
- 展示层
- 系统层
- 引用桥层

### 第二步：不要跨层误升级
常见错误：
- 展示层 ≠ 题面真值层
- 系统解锁层 ≠ 单关内容层
- 文本正文层 ≠ 已绑定到具体关卡编号

### 第三步：只把“引用桥层”当主推进线
如果当前发现不能增加 bridge/key，就只能算辅助发现。

## 什么叫“较强关卡线索”
以下信号更值得继续追：
1. 同时出现 `level/stage/mission` 标识和可读数据块
2. 同一对象或邻域里同时出现：
   - 关卡编号
   - 模板/对象 ID
   - 可读题面内容
3. Managed 类字段中出现：
   - `level`
   - `template`
   - `puzzle`
   - `story`
   - `clue`
   - `config`
   且能回查到资源层对象

## 什么叫“弱线索”
以下信号默认只算弱线索：
- 命名像 `LevelSomething`
- 文本里提到 story / mission / clue
- prefab 看起来像某类题面
- 图片/音效命名似乎有关卡语义

这些都可能有用，但不应自动升格为主结论。

## 推荐输出格式
当用户问“关卡资源在哪”时，优先这样回答：
1. **已确认的层**
   - 哪些文件属于布局层
   - 哪些文件属于正文层
   - 哪些文件属于展示层
   - 哪些文件属于系统层
2. **尚未闭环的缺口**
   - 还缺哪种 bridge/key
3. **下一步最合理动作**
   - 只给 1-3 个最值得做的动作

## 切线规则
当你连续两轮只能新增：
- 更多展示近邻
- 更多文本块
- 更多 theme / tutorial 配置

但没有新增任何 level -> object/document/runtime bridge，
就应该主动建议切线到：
- Managed / metadata
- 更底层序列化解析
- 单个强样本的 key 追踪
