# SuperPrompt - AI驱动的全栈应用开发自动化框架

## 项目简介

SuperPrompt 是一个基于 Claude AI 的全栈应用开发自动化框架，通过元提示词（Meta-Prompt）和多智能体协作系统，实现从需求分析到部署上线的全流程自动化开发。

## 核心特性

- **🤖 多智能体协作**: 基于专业分工的 AI 智能体团队（UX研究员、UI设计师、前端开发、后端架构师等）
- **📋 七阶段开发流程**: 从项目规划到部署上线的标准化开发流程
- **🔄 自动化工作流**: 支持任务并行调度、TDD循环、动态任务分配
- **📊 可追溯性**: 完整的任务记录和交接文档系统
- **🎨 设计到代码**: 从线框图、UI设计稿到高质量代码的完整转换
- **✅ 质量保证**: 内置测试驱动开发和自动化验收流程

## 项目结构

```
SuperPrompt/
├── README.md                   # 项目说明和环境配置指南
├── meta-prompt.md             # AI项目总监元提示词，定义整体开发流程
├── serena_init.md             # Serena MCP 初始化说明
├── docs/
│   └── task_format_spec.md    # 任务JSON结构规范
├── Prompts/
│   ├── stage-1.md             # 阶段1：项目规划与初始化
│   ├── stage-2.md             # 阶段2：核心规划与数据建模
│   ├── stage-3.md             # 阶段3：界面布局与API定义
│   ├── stage-4.md             # 阶段4：UI设计与微交互
│   ├── stage-5.md             # 阶段5：后端开发
│   ├── stage-6.md             # 阶段6：前端开发
│   └── stage-7.md             # 阶段7：部署与上线
└── back/                       # 历史备份文件（不包含在工作流程中）
```

## 开发流程

### 七阶段开发模型

1. **项目规划与初始化** - 分析需求、定义功能范围、初始化项目
2. **核心规划与数据建模** - 创建用户流程图和数据库架构
3. **界面布局与API定义** - 设计线框图和API规范
4. **UI设计** - 创建设计系统和高保真视觉稿
5. **后端开发** - 实现API端点和业务逻辑（TDD模式）
6. **前端开发** - 实现界面和交互（TDD模式）
7. **部署与上线** - 集成测试、CI/CD配置、生产部署

### 工作流特点

- **任务细化**: 使用 `sprint-prioritizer` 将宏观目标分解为可执行子任务
- **并行执行**: 支持单任务和多任务批处理模式
- **测试驱动**: 严格的 Red-Green-Refactor TDD循环
- **依赖管理**: 基于任务依赖关系的自动调度
- **状态追踪**: JSON格式的任务状态和历史记录

## 前置要求

### 必需软件

- **Node.js** (推荐 LTS 版本) - JavaScript 运行环境
- **Python 3.13 或更低版本** - 用于 Serena MCP 和相关工具
- **Git** - 版本控制系统
- **npm** 或 **yarn** - 包管理工具

### 必需账号

- **Anthropic Claude API** - AI 服务
- **智谱 GLM API** (可选) - 国内替代方案
- **Context7 API** (可选) - 最新文档检索
- **Supabase** (可选) - 数据库服务
- **Vercel/其他部署平台** (用于生产部署)

## 快速开始

### 1. 安装 Claude Code

## claude code的安装
- https://nodejs.org/en/download 安装指示安装node.js和npm工具
- npm install -g @anthropic-ai/claude-code

## 智谱glm4.6的配置
- curl -O "http://bigmodel-us3-prod-marketplace.cn-wlcb.ufileos.com/1753683727739-0b3a4f6e84284f1b9afa951ab7873c29.sh?ufileattname=claude_code_prod.sh"
- bash "1753683727739-0b3a4f6e84284f1b9afa951ab7873c29.sh"

## claude的hooks配置 目的是让claude执行完命令后给我们提示音
- 以windows为例，编辑C:\Users\<用户名>\.claude文件夹下的settings.json，添加以下内容(你需要创建hooks文件夹，并放入对应音频文件)：
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|MultiEdit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier -check ."
          }
        ]
      }
    ],
    "Stop": [
      {
        "matcher": "*",

        "hooks": [
          {
            "type": "command",
            "command": "powershell.exe -c \"(New-Object Media.SoundPlayer 'C:\\\\Users\\\\Administrator\\\\.claude\\\\hooks\\\\马里奥吃金币.wav').PlaySync()\""
          }
        ]
      }
    ],

    "Notification": [
      {
        "matcher": "*",

        "hooks": [
          {
            "type": "command",

            "command": "powershell.exe -c \"(New-Object Media.SoundPlayer 'C:\\\\Users\\\\Administrator\\\\.claude\\\\hooks\\\\马里奥吃蘑菇音效.wav').PlaySync()\""
          }
        ]
      }
    ]
  }
## MCP 安装
### serena 提供代码高效检索工具，节约agent使用的token
- 确保系统中安装了3.13及以下版本的python，且是默认使用的python，然后执行pip install uv,安装uv和uvx工具
- claude mcp add serena -- uvx --from git+https://github.com/oraios/serena serena start-mcp-server --context ide-assistant --project $(pwd) 这个命令是项目级别的，新项目都需要执行

### 智谱视觉+网络搜索MCP
- claude mcp add zai-mcp-server --env Z_AI_API_KEY=<智谱token> -- npx -y @z_ai/mcp-server
- claude mcp add -s user -t http web-search-prime https://open.bigmodel.cn/api/mcp/web_search_prime/mcp --header "Authorization: Bearer <智谱token>"

### Context7 最新api文档检索工具，节约token，提供最新文档
- claude mcp add --transport http context7 https://mcp.context7.com/mcp --header "CONTEXT7_API_KEY: <CONTEXT7的token>"

### chrome浏览器mcp
claude mcp add chrome-devtools -s user -- npx chrome-devtools-mcp@latest

### playwright mcp 已被chrome-devtools代替，可以不装
claude mcp add playwright -s user -- npx @playwright/mcp@latest

### supabase 线上supabase数据库连接mcp
- claude mcp add supabase -s local -e SUPABASE_ACCESS_TOKEN=<supabase用户的token> -- npx -y @supabase/mcp-server-supabase@latest

### shadcn-ui shadcn/ui的mcp服务器，可以查看最新的组件信息和使用说明
- claude mcp add shadcn-ui -s user -- npx @jpisnice/shadcn-ui-mcp-server --github-api-key <github用户token>

安装完成后使用claude mcp list查看安装是否成功，可以用claude mcp remove <mcp名称> 来删除mcp

#### shadcn/ui配置-next.js
- 在用户级或项目级的CLAUDE.md中添加以下提示词：
  - “当项目准备使用shadcn作为UI库时，在项目初始化之后，需要调用npx shadcn@latest init进行shadcn初始化；shadcn初始化完成之后，增加ui组件的命令参考：npx shadcn@latest add button
使用规则：当被要求使用 shadcn 组件时，请使用shadcn-ui MCP。
规划规则：当被要求规划任何与 shadcn 相关的使用方式时，使用shadcn-ui MCP，在适用组件的地方应用组件，尽可能使用整个模块（例如，登录页面、日历）
实施规则：实施时，首先调用 demo tool查看其如何使用，然后implement以确保其正确implemented，同时安装组件。请勿自行编写文件。”


## agent的安装
- 通过github等途径下载agent文件放入用户或项目级的.claude/agents文件夹中
- https://github.com/contains-studio/agents/tree/main

## 通过bamd方法生成需求文档，UX文档，架构文档
- https://github.com/bmad-code-org/BMAD-METHOD
- 可以直接在claude中执行bmad方法，也可以把https://github.com/bmad-code-org/BMAD-METHOD/blob/main/dist/teams/team-fullstack.txt 文件拿给Gpt或Gemini线上网页在线生成

## 项目初始化的任务执行
### 项目初始化
- 使用项目脚手架脚本创建好项目文件夹后，进入项目文件夹，把项目的需求文档，UX文档，架构文档放进里面的docs文件夹，在项目文件夹运行claude，在claude里执行/init，生成CLAUDE.MD
### serana启动
- uvx --from git+https://github.com/oraios/serena serena project index 这个命令是项目级别的，新项目都需要执行
- claude中执行/mcp__serena__initial_instructions，如果无法执行这个命令，就要求“读取 Serena 的初始说明”
- 把上一步执行后得到的提示词存入项目里的CLAUDE.MD中，可以直接复制我提供的serena_init.md文件中的内容
### shadcn-ui启动
- 在项目脚手架工具，比如'create next-app'，初始化项目完成后，执行npx shadcn@latest init
- 在生成的Components.json文档里，添加以下注册信息
  -   "registries": {
    "@aceternity": "https://ui.aceternity.com/registry/{name}.json",
    "@originui": "https://originui.com/r/{name}.json",
    "@cult": "https://cult-ui.com/r/{name}.json",
    "@kibo": "https://www.kibo-ui.com/r/{name}.json",
    "@reui": "https://reui.io/r/{name}.json",
    "@magicui": "https://magicui.design/r/{name}.json",
    "@kokonutui": "https://kokonutui.com/r/{name}.json"
  }



## 启动claude code的命令
claude --dangerously-skip-permissions

## 更新claude code的命令
npm i -g @anthropic-ai/claude-code
## 使用Prompt执行开发
- Prompts文件夹下每个stage按阶段执行，你需要确保每个指令执行完的内容是你预期的，如果有代码，执行的时候没报错。
- 慢即是快，garbage in garbage out，只有这样一步一个脚印，做出来的东西才是你想要的。
- 养成习惯，每个阶段指令完成就用git版本管理
- 注意审查每一阶段指令1生成的任务清单，如有必要，请人工修改，不要让计划里包含指令1里要求的任务范围之外的任务。
- 注意审查每一阶段指令2生成的结果，特别是UX/UI，确保与预期相符，如果不符预期，则要让相应agent修改。
### 这份Prompt是怎么生成的
- github上找到想要用的agent库，使用 https://gitingest.com/ 提取库的详细信息，保存到agent_data.md中
- 把agent_data.md，prd.md，front-end-spec.md，fullstack-architecture.md（BMAD方法生成）丢给Gemini，然后把meta-prompt.md中的内容作为提示词，Gemini就生成了一个初版
- 我再人工在初版上调整