## claude code的安装
- https://nodejs.org/en/download 安装指示安装node.js和npm工具
- npm install -g @anthropic-ai/claude-code

## 智谱glm4.5的配置
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
- uvx --from git+https://github.com/oraios/serena serena project index 这个命令是项目级别的，新项目都需要执行
- claude mcp add serena -- uvx --from git+https://github.com/oraios/serena serena start-mcp-server --context ide-assistant --project $(pwd) 这个命令是项目级别的，新项目都需要执行

### 智谱视觉+网络搜索MCP
- claude mcp add zai-mcp-server --env Z_AI_API_KEY=<智谱token> -- npx -y @z_ai/mcp-server
- claude mcp add -s user -t http web-search-prime https://open.bigmodel.cn/api/mcp/web_search_prime/mcp --header "Authorization: Bearer <智谱token>"

### Context7 最新api文档检索工具，节约token，提供最新文档
- claude mcp add --transport http context7 https://mcp.context7.com/mcp --header "CONTEXT7_API_KEY: <CONTEXT7的token>"

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
- claude中执行/mcp__serena__initial_instructions，如果无法执行这个命令，就要求“读取 Serena 的初始说明”
- 把上一步执行后得到的提示词存入项目里的CLAUDE.MD中
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


## 使用Prompt执行开发
- FinalPrompt.md
### 这份Prompt是怎么生成的
- github上找到想要用的agent库，使用 https://gitingest.com/ 提取库的详细信息，保存到agent_data.md中
- 把agent_data.md，prd.md，front-end-spec.md，fullstack-architecture.md（BMAD方法生成）丢给Gemini，然后把meta-prompt.md中的内容作为提示词，Gemini就生成了一个初版
- 我再人工在初版上调整