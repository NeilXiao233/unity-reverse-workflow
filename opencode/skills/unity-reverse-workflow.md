# unity-reverse-workflow

通用 Unity 逆向协作 workflow，适用于 OpenCode 场景。

## 触发场景
只要用户在问：
- Unity 逆向
- 提取图片 / 图标 / 立绘
- 提取音效 / BGM / 语音
- 提取关卡 / 文本 / 配置
- sharedassets / AssetBundle / AssetRipper 导出层 / metadata / Managed 分析

就应优先使用本 workflow。

## 方法论
把 Unity 逆向任务拆成：
1. 容器层
2. 对象层
3. 语义层
4. 引用层
5. 闭环层

原则：
- 先盘点，后提取
- 先样本，后批量
- 先找对象级入口，再谈闭环

## 默认工具策略
优先使用已有工具：
- `UnityPy`
- `python3`
- `dotnet`
- `strings`
- `xxd`
- `hexdump`

如果工具缺失：
- 先说明已有工具还能做什么
- 再按需引导安装
- 不要因为缺一个工具就完全停住

参考：
- `../../shared/references/tool-installation.md`
- `../../shared/references/unitypy-examples.md`
- `../../shared/references/audio-extraction.md`
- `../../shared/references/level-hunting.md`

## 输出要求
每次输出都尽量包含：
1. 当前目标
2. 已确认事实
3. 不能直接下结论的点
4. 下一步建议

## 注意
- 不要假定项目目录结构
- 不要把展示层近邻误写成关卡真值
- 不要把 metadata 字符串命中误写成字段级闭环
