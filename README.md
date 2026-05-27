# Knowledge Graph Creator — 知识图谱生成器

一个为 TRAE SOLO 打造的**代码知识图谱 Skill**，自动扫描项目文件、分析关联关系、生成交互式 D3.js 力导向图 HTML。

## 能做什么？

只需说一句话，就能把整个项目的代码结构变成一张可交互的关系图：

> "帮我创建这个项目的知识图谱"

AI 会自动：
1. 扫描项目所有源代码和文档
2. 分析 import、extends、implements 等关联关系
3. 生成交互式 HTML 知识图谱（双击即用）

## 支持的文件类型

Python / JavaScript / TypeScript / Java / Go / Rust / Markdown / JSON / YAML / Vue / PHP / Ruby / C/C++ / C# / Kotlin / Swift 等 20+ 种格式。

自动跳过 `.git`、`node_modules`、`dist`、`build`、`.venv` 等隐藏目录和无关目录。

## 输出效果

![知识图谱示意图](https://d3js.org/logo.svg)

生成的 HTML 知识图谱包含 **10 项交互功能**：
- 力导向布局，节点自动排布
- 鼠标滚轮缩放 + 拖拽平移
- 节点拖拽固定位置
- 悬停邻居高亮
- 名称搜索定位
- 节点详情面板
- 分组图例
- 边关系标签
- 侧边栏节点列表
- 统计信息（节点数/边数/模块数）

## 安装方法

1. 打开 TRAE / TRAE SOLO
2. 进入 **设置 → 规则与技能 → 技能**
3. 点击 **创建 → 导入文件**
4. 选择本仓库的 `SKILL.md` 文件

或通过终端：
```bash
mkdir -p ~/.trae-cn/skills/knowledge-graph-creator/
cp SKILL.md ~/.trae-cn/skills/knowledge-graph-creator/
```

## 使用方法

安装后在 SOLO 模式直接提问：

> "帮我创建这个项目的知识图谱"

> "看看 Controller 和 Service 之间怎么关联的"

> "分析架构关系，输出 HTML 图"

> "帮我画个依赖关系图"

## 创作背景

这是为 **TRAE SOLO 技能创作赛** 准备的作品。核心思路是：**让 AI 不仅能理解代码，还能把代码之间的关系可视化出来**。

传统查看项目架构需要手动翻文件、画架构图，每次项目变动就要重画。这个 Skill 把整个过程自动化了——输入自然语言，输出交互式图谱。

## 许可

MIT
