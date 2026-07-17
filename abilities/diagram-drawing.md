# 图表绘制能力 (Diagram Drawing Ability)

## 概述

本能力提供了使用 draw.io、Mermaid、PlantUML 等工具在 GitHub 项目中创建、管理和协作图表的完整指导。包括流程图、架构图、ER 图、UML 图、思维导图等多种图表类型的创建和维护。

---

## 目的

- 帮助开发团队快速创建技术图表并集成到项目文档中
- 建立图表版本控制和协作审查流程
- 提供多种图表工具的最佳实践
- 确保图表与代码同步更新

---

## 核心能力

### 1. 图表类型识别与选择

#### 1.1 流程图 (Flowchart)
用于表示程序流程、业务流程或决策树。

**工具推荐**: draw.io, Mermaid, Lucidchart
**特点**: 使用菱形表示决策，矩形表示步骤，箭头表示流向
**适用场景**: API 调用流程、用户登录流程、数据处理流程

#### 1.2 架构图 (Architecture Diagram)
展示系统的高层结构和组件关系。

**工具推荐**: draw.io, Excalidraw, Lucidchart
**特点**: 包含组件、服务、数据库、外部系统等
**适用场景**: 微服务架构、系统设计、基础设施布局

#### 1.3 ER 图 (Entity-Relationship Diagram)
展示数据库表和关系。

**工具推荐**: draw.io, dbdiagram.io, Lucidchart
**特点**: 展示实体、属性和关系
**适用场景**: 数据库设计、数据模型规划

#### 1.4 UML 图 (Unified Modeling Language)
标准化的系统建模方式。

**工具推荐**: draw.io, PlantUML, StarUML
**特点**: 类图、时序图、用例图等多种形式
**适用场景**: 软件设计、类关系描述、时序交互

#### 1.5 思维导图 (Mind Map)
用于头脑风暴和概念组织。

**工具推荐**: draw.io, Miro, XMind
**特点**: 树形结构，中心主题向外扩展
**适用场景**: 项目规划、需求分析、知识整理

#### 1.6 甘特图 (Gantt Chart)
项目时间管理和任务调度。

**工具推荐**: draw.io, Miro, Monday.com
**特点**: 任务、时间线、依赖关系
**适用场景**: 项目时间规划、里程碑管理

---

## 工作流程

### 工作流程 1: 使用 draw.io 创建并保存到 GitHub

#### 步骤 1: 打开 draw.io 并创建图表

```
1. 访问 https://app.diagrams.net/
2. 选择创建新文件或打开现有文件
3. 选择保存位置（GitHub、Google Drive、OneDrive 等）
4. 授权 GitHub 账户访问权限
```

#### 步骤 2: 绘制图表

```
1. 从左侧面板选择形状和连接器
2. 拖拽放置到画布
3. 使用右侧面板调整样式、颜色、文字
4. 添加连接线和标签
5. 使用图层管理复杂图表
```

#### 步骤 3: 保存到 GitHub

```
1. File → Save As
2. 选择 GitHub 作为存储位置
3. 授权并选择仓库
4. 选择分支（通常是 main 或 develop）
5. 指定文件路径（如 docs/diagrams/architecture.drawio）
6. 提交并添加提交信息
```

#### 步骤 4: 导出为图片

```
1. File → Export As → PNG/SVG/PDF
2. 选择合适的分辨率和格式
3. 保存文件到本地或直接上传到 GitHub
4. 在 README 或文档中引用
```

#### 步骤 5: 在 Markdown 中嵌入

```markdown
![系统架构](docs/diagrams/architecture.png)

或使用 HTML 获得更多控制：

<img src="docs/diagrams/architecture.svg" alt="系统架构" width="600">
```

---

### 工作流程 2: 使用 Mermaid 进行版本控制友好的图表

Mermaid 是一种文本格式的图表描述语言，天生适合 Git 版本控制。

#### 步骤 1: 在 Markdown 文件中编写 Mermaid

```markdown
# 系统架构

## 服务架构图

\`\`\`mermaid
graph TD
    Client[客户端]
    Gateway[API 网关]
    AuthService[认证服务]
    UserService[用户服务]
    DB[(数据库)]
    Cache[(缓存)]
    
    Client -->|请求| Gateway
    Gateway -->|验证| AuthService
    Gateway -->|查询| UserService
    AuthService --> Cache
    UserService --> DB
    UserService --> Cache
\`\`\`

## 时序图

\`\`\`mermaid
sequenceDiagram
    用户->>客户端: 登录
    客户端->>API网关: 发送认证请求
    API网关->>认证服务: 验证凭证
    认证服务->>数据库: 查询用户
    认证服务-->>API网关: 返回 Token
    API网关-->>客户端: 返回 Token
    客户端-->>用户: 登录成功
\`\`\`
```

#### 步骤 2: GitHub 原生支持

- GitHub 会自动在 README 中渲染 Mermaid 图表
- 支持所有 Markdown 文件
- 完全版本控制友好

#### 步骤 3: 提交更新

```bash
git add docs/ARCHITECTURE.md
git commit -m "docs: update system architecture diagram"
git push origin main
```

---

### 工作流程 3: Pull Request 中的图表审查

#### 审查流程

1. **创建分支**
```bash
git checkout -b docs/update-architecture-diagram
```

2. **在分支中更新图表**
```bash
# 编辑 Mermaid 图表或上传新的 .drawio 文件
vi docs/architecture.md
```

3. **提交变更**
```bash
git add docs/
git commit -m "docs: refactor architecture diagram for clarity"
git push origin docs/update-architecture-diagram
```

4. **创建 Pull Request**
- 标题: `docs: refactor architecture diagram for clarity`
- 描述: 
  ```markdown
  ## 变更内容
  - 将三层架构更新为微服务架构
  - 添加了服务间的通信方式说明
  - 优化了图表的可读性
  
  ## 原因
  之前的架构图已不符合当前系统设计
  
  ## 审查焦点
  - 架构是否准确反映当前系统设计
  - 图表清晰度和易理解性
  - 是否遗漏了关键组件
  ```

5. **审查和反馈**
- 团队成员在 PR 中审查
- 可以对具体行进行注释
- 如有需要，提出修改建议

6. **合并**
- 获得审批后合并到主分支
- 自动部署到文档网站（如使用 GitHub Pages）

---

## 工具对比与选择

### draw.io (diagrams.net)

| 特性 | 详情 |
|------|------|
| **优点** | 免费、功能完整、支持多种图表类型、GitHub 集成 |
| **缺点** | XML 格式不易文本 diff |
| **文件格式** | .drawio (XML), 可导出 PNG/SVG |
| **版本控制** | 支持，但 XML diff 不直观 |
| **协作** | 通过 GitHub PR 进行 |
| **最佳用于** | 复杂的流程图、架构图、ER 图 |

### Mermaid

| 特性 | 详情 |
|------|------|
| **优点** | 文本格式、原生 Git 支持、GitHub 原生渲染、Markdown 集成 |
| **缺点** | 功能相对简单、某些复杂图表难以实现 |
| **文件格式** | Markdown 文本 |
| **版本控制** | 完全支持、diff 非常清晰 |
| **协作** | GitHub PR 天然支持 |
| **最佳用于** | 流程图、时序图、类图、状态图、甘特图 |

### PlantUML

| 特性 | 详情 |
|------|------|
| **优点** | 专业的 UML 支持、文本格式、强大的 IDE 插件 |
| **缺点** | 学习曲线陡、需要编译工具链 |
| **文件格式** | .puml 文本格式 |
| **版本控制** | 完全支持 |
| **协作** | GitHub PR 支持 |
| **最佳用于** | UML 图、类设计、复杂系统建模 |

### Excalidraw

| 特性 | 详情 |
|------|------|
| **优点** | 手绘风格、现代 UI、GitHub 集成 |
| **缺点** | 功能相对新、生态还在发展中 |
| **文件格式** | .excalidraw (JSON) |
| **版本控制** | 支持，JSON diff 可读性一般 |
| **协作** | GitHub PR、实时协作链接 |
| **最佳用于** | 快速原型、架构概念图、白板风格设计 |

---

## 最佳实践

### BP1: 文件组织结构

```
project-repo/
├── docs/
│   ├── diagrams/
│   │   ├── architecture/
│   │   │   ├── system-architecture.drawio
│   │   │   ├── system-architecture.png
│   │   │   └── system-architecture.md
│   │   ├── database/
│   │   │   ├── schema.drawio
│   │   │   └── schema.png
│   │   └── workflows/
│   │       ├── auth-flow.mermaid
│   │       └── deployment.mermaid
│   ├── ARCHITECTURE.md
│   └── DATABASE.md
├── .diagrams.config
└── README.md
```

### BP2: 文件命名规范

```
# Mermaid 文件
{domain}-{type}.mermaid
示例: user-auth-flow.mermaid, deployment-timeline.mermaid

# draw.io 文件
{domain}-{type}.drawio
示例: system-architecture.drawio, database-schema.drawio

# 导出的图片
{domain}-{type}.{format}
示例: system-architecture.png, database-schema.svg
```

### BP3: 文档集成

#### 在 README 中嵌入

```markdown
# 项目架构

![系统架构图](docs/diagrams/architecture/system-architecture.png)

详见 [架构文档](docs/ARCHITECTURE.md)
```

#### 创建专门的文档文件

```markdown
# 系统架构文档

## 整体设计

\`\`\`mermaid
graph TD
    ...
\`\`\`

## 各层说明

### 表现层
...

### 业务逻辑层
...

### 数据层
...
```

### BP4: 版本管理

#### 对于 Mermaid (推荐方式)

```yaml
# .github/workflows/mermaid-check.yml
name: Validate Mermaid Diagrams

on: [pull_request]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Validate Mermaid syntax
        run: npm install -g mermaid-cli && mmdc -i docs/diagrams/*.mermaid
```

#### 对于 draw.io

1. 定期导出为 PNG/SVG，与 .drawio 文件一起提交
2. PNG/SVG 用于展示，.drawio 用于编辑
3. 在 CI/CD 中检查两者是否同步

```bash
# 脚本示例: scripts/sync-diagrams.sh
#!/bin/bash
for file in docs/diagrams/*.drawio; do
  name="${file%.drawio}"
  echo "Exporting $file to $name.png"
  # 使用 draw.io CLI 导出
  # docker run --rm -v $(pwd):/data draw.io:latest -x -f png $file -o $name.png
done
```

### BP5: 审查清单

图表 PR 审查时的检查项：

- [ ] **准确性**: 图表是否准确反映当前系统/流程状态？
- [ ] **完整性**: 是否遗漏了关键组件或步骤？
- [ ] **清晰性**: 图表易于理解吗？命名是否清晰？
- [ ] **一致性**: 是否与其他文档保持一致？
- [ ] **可维护性**: 格式选择是否有利于未来更新？
- [ ] **导出**: 是否包含了图片导出供预览？

### BP6: 更新触发点

何时应更新图表：

1. 系统架构变更
2. 新增主要功能或服务
3. 数据库 Schema 变更
4. 工作流程优化
5. 部署流程改变
6. API 接口升级
7. 年度/版本发布

### BP7: 工具链集成

#### GitHub Action 自动化

```yaml
# .github/workflows/diagram-export.yml
name: Auto Export Diagrams

on:
  push:
    paths:
      - 'docs/diagrams/*.drawio'

jobs:
  export:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Export diagrams
        run: |
          # 使用 draw.io CLI 工具导出
          docker run --rm -v $(pwd):/data draw.io:latest \
            -x -f png docs/diagrams/*.drawio
      - name: Commit changes
        run: |
          git add docs/diagrams/*.png
          git commit -m "docs: auto-export diagrams" || true
          git push
```

---

## 常见场景指南

### 场景 1: 创建首个系统架构图

1. 在 draw.io 中选择 "System Design" 模板
2. 根据实际系统修改组件
3. 标注数据流和通信方式
4. 导出为 PNG，保存原始 .drawio 文件到 GitHub
5. 在 docs/ARCHITECTURE.md 中嵌入

### 场景 2: 更新数据库设计

1. 打开现有的 schema.drawio
2. 修改表结构或添加新表
3. 在 PR 中提交，请求 DBA 或架构师审查
4. 同时更新数据库迁移脚本和文档
5. 导出新的 PNG，替换旧版本

### 场景 3: 记录新的工作流程

1. 使用 Mermaid 的 `flowchart` 或 `graph` 语法
2. 在 docs/workflows/ 目录创建 .mermaid 文件
3. 在相关文档中引用
4. 提交 PR，团队审查流程逻辑

### 场景 4: 团队协作编辑图表

1. 使用 draw.io 的在线协作功能或分支策略
2. 一个人创建图表初版
3. 其他人通过 PR 提议修改
4. 讨论后再进行更新
5. 合并时记录决策和原因

---

## 故障排除

### 问题 1: draw.io 文件在 GitHub 中无法编辑

**原因**: GitHub 不原生支持 .drawio 文件编辑
**解决方案**:
- 使用 drawio-desktop 本地编辑后提交
- 或在 https://app.diagrams.net/ 打开，选择 "GitHub" 作为打开位置
- 或使用浏览器扩展在线编辑

### 问题 2: Mermaid 图表在某些渲染器上不支持

**原因**: 不同的 Mermaid 版本支持的语法不同
**解决方案**:
- 使用更基础的图表类型
- 检查 GitHub 支持的 Mermaid 版本
- 使用 `mmdc` 本地测试渲染

### 问题 3: 图表文件 diff 过大，难以审查

**原因**: XML 或 JSON 格式的图表文件修改时产生大量 diff
**解决方案**:
- 为二进制格式设置 .gitattributes
- 或导出为图片进行视觉对比
- 或改用 Mermaid 等文本格式
- 在 PR 描述中附加图表快照

---

## 工具命令参考

### draw.io CLI

```bash
# 导出为 PNG
draw.io -x input.drawio -o output.png

# 导出为 SVG
draw.io -x input.drawio -o output.svg -f svg

# 导出为 PDF
draw.io -x input.drawio -o output.pdf -f pdf

# 指定尺寸
draw.io -x input.drawio -o output.png -w 1920 -h 1080
```

### Mermaid CLI

```bash
# 安装
npm install -g mermaid-cli

# 导出为 PNG
mmdc -i diagram.mermaid -o diagram.png

# 导出为 SVG
mmdc -i diagram.mermaid -o diagram.svg -t svg

# 导出为 PDF
mmdc -i diagram.mermaid -o diagram.pdf

# 指定配置
mmdc -i diagram.mermaid -o diagram.png -c config.json
```

### PlantUML CLI

```bash
# 安装
npm install -g plantuml

# 生成 PNG
plantuml diagram.puml

# 生成 SVG
plantuml -tsvg diagram.puml

# 生成 PDF
plantuml -tpdf diagram.puml
```

---

## 相关资源

- **draw.io 官方**: https://www.diagrams.net/
- **Mermaid 文档**: https://mermaid.js.org/
- **PlantUML 文档**: https://plantuml.com/
- **GitHub Mermaid 支持**: https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-diagrams
- **Excalidraw**: https://excalidraw.com/

---

## 更新历史

| 版本 | 日期 | 变更 |
|------|------|------|
| 1.0 | 2026-07-17 | 初始版本，包含 draw.io、Mermaid、PlantUML 工作流程 |
