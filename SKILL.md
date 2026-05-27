---
name: knowledge-graph-creator
description: 当用户要求创建知识图谱、架构关系图、代码依赖分析图、模块关系可视化时使用。自动扫描项目中的代码和文档，分析文件间的关联关系，输出交互式 D3.js 力导向图 HTML 文件。支持暗色主题、节点搜索、邻居高亮、信息面板联动。
---

# Knowledge Graph Creator — 知识图谱生成器

## 概述

扫描项目中的可读文本文档（源代码、配置文件、文档等），自动分析文件之间的关联关系，输出交互式 D3.js 力导向图 HTML 文件。

用户只需说"帮我创建项目知识图谱"或"看看模块之间的关系"，即可自动完成从扫描到可视化的全流程。

## 触发条件

用户说以下任意话时自动调用此 Skill：

- "创建知识图谱"
- "生成模块关系图"
- "看看代码结构之间怎么关联的"
- "分析项目架构关系"
- "帮我画个依赖关系图"
- "可视化项目模块间的关系"
- "生成知识图谱 HTML"

---

## 工作流程

### Step 1：扫描项目文件

扫描项目根目录下所有可读文本文件（支持 py / js / ts / java / go / rs / md / json / yaml / vue 等 20+ 种格式），跳过隐藏目录（以 `.` 开头）、node_modules、vendor、dist、build、__pycache__、target 等无关目录。

### Step 2：分析文件关联关系

对扫描到的文件，自动分析以下维度的关系：

| 关系类型 | 检测方式 | 示例 |
|---------|---------|------|
| **导入/引用** | import / require / include | `import { Component } from './Component'` |
| **继承** | extends / implements | `class Service extends BaseService` |
| **接口实现** | implements | `class Controller implements IController` |
| **文件引用** | 配置文件互相引用，文档链接 | `config.yaml include: base.yaml` |
| **目录归属** | 目录结构反映的模块关系 | `controllers/` 下所有文件属于 Controller 模块 |
| **数据流** | Controller → Service → DAO/Entity | Controller 调用 Service, Service 调用 Repository |

**策略**：小项目（< 50 个文件）逐文件读取分析；大项目使用 grep/ripgrep 提取 import 关系，不读取完整文件内容。

### Step 3：构建图数据结构

将分析结果组织为 nodes + edges 的 JSON 格式。节点按功能分组（controller / service / entity / config / util / middleware / component 等 19 种分组），每种分组有独立颜色。

### Step 4：生成交互式 HTML

生成自包含的 HTML 文件，核心使用 D3.js v7 力导向图渲染。

## HTML 输出规格

### 技术栈

| 类别 | 技术/库 | 作用 |
|------|---------|------|
| 核心可视化 | D3.js v7 | 力导向图布局、节点/边绘制、缩放/拖拽、动态更新 |
| 页面样式 | HTML5 + CSS3 | 侧边栏、工具提示、图例、响应式布局（暗色主题） |
| 交互功能 | 原生 JavaScript | 节点搜索、信息面板联动、缩放控制、邻居高亮 |
| 数据格式 | JSON | 硬编码的 nodes 和 edges 数据 |

### 交互功能（10 项）

1. **力导向布局** — 节点自动排布，同类聚集，拖拽后自动复位
2. **缩放/拖拽** — 鼠标滚轮缩放，空白区域拖拽平移画布
3. **节点拖拽** — 单个节点可拖拽，松开后固定位置
4. **邻居高亮** — 悬停节点时，关联节点和边高亮，其他淡出
5. **节点搜索** — 输入名称模糊搜索，定位并居中到目标节点
6. **信息面板** — 点击节点显示详情（文件路径、文件类型、功能描述）
7. **图例** — 按 group 着色，显示颜色和数量统计
8. **边标签** — 悬停边时显示关系类型（import / extends / call 等）
9. **菜单列表** — 侧边栏列出所有节点，点击可聚焦
10. **统计信息** — 顶部显示节点数、边数、模块数

### 颜色方案（暗色主题）

| 分组 | 色值 | 分组 | 色值 |
|------|------|------|------|
| controller | `#22d3ee` 青色 | service | `#34d399` 翠绿 |
| entity/model | `#a78bfa` 紫色 | repository/dao | `#fb923c` 橙色 |
| config | `#fbbf24` 琥珀 | util/helper | `#fb7185` 粉红 |
| middleware | `#818cf8` 靛蓝 | route | `#2dd4bf` 青绿 |
| component | `#c084fc` 粉紫 | page/view | `#f43f5e` 玫瑰 |
| store | `#eab308` 黄色 | doc | `#94a3b8` 灰色 |
| test | `#ef4444` 红色 | other | `#64748b` 浅灰 |

## 输出文件结构

```
project-root/
├── knowledge-graph/
│   ├── index.html       # 自包含交互式知识图谱（双击即用）
│   └── data.json        # 提取的图数据（可选，方便复查）
```

`index.html` 是**自包含**的——所有 CSS、JS（D3.js 通过 CDN 加载）和 JSON 数据都内嵌在单个 HTML 中，用户双击就能在浏览器打开。

## 常见问题

1. **图太乱**：节点 > 100 时自动按模块聚合，只显示模块级关系
2. **没有 import 关系**：纯文档项目按目录层级+文档内部链接构建
3. **找不到关联**：至少按目录结构生成"包含关系"（目录包含文件）
4. **大文件**：跳过 > 1MB 的超大文件和二进制文件
5. **CDN 失败**：使用 `https://d3js.org/d3.v7.min.js`

## 验证清单

- [ ] 扫描了项目所有可读文件，排除了隐藏目录和无关目录
- [ ] import/require 关系被正确提取
- [ ] 节点按 group 正确着色
- [ ] 力导向图布局流畅，节点不重叠
- [ ] 缩放、拖拽、搜索功能正常工作
- [ ] 邻居高亮交互响应正常
- [ ] 信息面板显示正确的节点详情
- [ ] HTML 文件自包含，双击可打开
- [ ] 暗色主题无误
