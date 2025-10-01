### **第一阶段：项目规划与初始化 (Project Planning & Initialization)**

**指令1：执行项目规划与初始化**

**1. 阶段目标:**
分析所有输入文档，生成项目的核心规划文件，并创建项目的基础骨架和协作目录，为后续的AI智能体团队提供一个标准化的工作环境。  
**2. 具体任务:**

1. **读取并分析以下项目文件：**  
   * `docs/prd.md` (产品需求文档)  
   * `docs/front-end-spec.md` (前端技术规格)  
   * `docs/fullstack-architecture.md` (全栈技术架构)  
2. **定义功能范围 (Define Feature Scope):**  
   * 基于 `docs/prd.md`，精确提炼出一个完整的、详细的功能列表。每个功能点都应清晰、无歧义。  
   * 将此列表保存到 `docs/feature-scope.md`。此文件将作为项目范围的最终依据，所有后续阶段都必须严格遵守，不得超出此范围。  
3. **确认技术栈 (Confirm Tech Stack):**  
   * 基于 `docs/fullstack-architecture.md`，列出项目将使用的完整技术栈，包括语言、前后端框架、UI库、数据库和部署平台。  
   * 将此列表保存到 docs/tech-stack.md。  
4. **生成脚手架指南 (Generate Scaffolding Guide):**  
   * 基于 `docs/tech-stack.md` 的内容，查阅脚手架工具官网文档，进行分析。
   * 生成一份详细的 `scaffold-guide.md` 文件，其中应包含用于初始化项目脚手架的、确切的、可执行的命令行指令，以及依赖安装的说明。
   * 将文件保存到 `docs/scaffold-guide.md`,如果其中有交互式配置，比如t3-app、shadcn的配置则指导用户如何执行。
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
## 用户操作
**执行项目初始化:**
  * 阅读 `docs/scaffold-guide.md` 文件，确认项目初始化所需的命令和配置，并执行命令，比如npm create t3-app@latest do-j-pm，这会创建项目文件夹，并执行npx shadcn@latest init初始化shadcn组件库，并调整shadcn的component.json里的注册信息，然后add shadcn的组件。
  * **交互式配置指导:** 由于此命令是交互式的，需要根据 `docs/scaffold-guide.md` 中的“配置选项”部分进行选择。
  * 把当前目录下的docs文件夹放进项目文件夹中。
  * 进入项目文件夹，重新启动claude,执行claude和serena的初始化操作。详见README.md。
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
**指令2: 初始化项目并配置环境 (Initialize Project & Configure Environment):**

* **目标:** 根据 `docs/scaffold-guide.md` 初始化项目，并引导用户完成关键配置。
* **智能体:** `project-manager`
* **任务:**
  1. **安装依赖与环境配置:**
     * **生成env配置文件提醒用户:**
       * 生成env.example和env.local两个文件
       * **暂停并明确提示用户:** "项目依赖已安装。现在，请您打开 `do-j-pm/.env.local` 文件，并根据 `docs/scaffold-guide.md` 的指导，填入您自己的数据库、Supabase、vercel和API密钥等敏感信息。**这是项目运行的关键步骤。** "
     * 根据 `docs/scaffold-guide.md` 初始化项目(我已经使用脚手架工具完成了项目初始配置和shadcn的配置,你需要完成剩余操作，如果有需要交互式配置的操作，你不要直接执行，而是把命令告诉用户，用户执行之后，告诉你继续你再进行后续操作)
  2. **设计文件结构 (Design File Structure):**
     * 在项目初始化并安装依赖后，基于 `docs/fullstack-architecture.md`和`docs/scaffold-guide.md`中定义的结构和现有项目初始化后达代码文件结构，设计一个详细的目录和文件结构。
     * 此结构必须包含用于存放各阶段记录的 Worknotes 文件夹，并将设计稿、文档、Prompt等内容明确分区。
     * 将设计的目录树结构保存到 `docs/project-structure.md`。
  3. **应用文件结构并创建协作目录 (Apply File Structure & Create Collab Dirs):**
     * **应用结构:** 读取 `docs/project-structure.md`，在 `do-j-pm` 目录内生成并执行一个脚本，通过 `mkdir` 和 `mv` 等命令，将项目调整为 `project-structure.md` 中定义的结构。
     * **创建协作目录:** 确保 `designs`、`Worknotes` 等协作目录已根据 `project-structure.md` 创建。

     # 创建工作记录目录，并为每个阶段生成独立的、包含初始结构的JSON文件
     mkdir -p Worknotes
     echo {"tasks": [], "summary": "", "next_stage_suggestions": ""} > Worknotes/stage-1-planning-and-initialization.json
     echo {"tasks": [], "summary": "", "next_stage_suggestions": ""} > Worknotes/stage-2-core-planning.json
     echo {"tasks": [], "summary": "", "next_stage_suggestions": ""} > Worknotes/stage-2-data-modeling.json
     echo {"tasks": [], "summary": "", "next_stage_suggestions": ""} > Worknotes/stage-3-interface-layout.json
     echo {"tasks": [], "summary": "", "next_stage_suggestions": ""} > Worknotes/stage-3-api-definition.json
     echo {"tasks": [], "summary": "", "next_stage_suggestions": ""} > Worknotes/stage-4-ui-design.json
     echo {"tasks": [], "summary": "", "next_stage_suggestions": ""} > Worknotes/stage-4-ui-whimsy.json
     echo {"tasks": [], "summary": "", "next_stage_suggestions": ""} > Worknotes/stage-5-backend-development.json
     echo {"tasks": [], "summary": "", "next_stage_suggestions": ""} > Worknotes/stage-6-frontend-development.json
     echo {"tasks": [], "summary": "", "next_stage_suggestions": ""} > Worknotes/stage-7-integration-deployment-and-launch.json

     echo "协作目录结构与工作记录JSON文件创建完成。"

————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
## 用户操作
  * 阅读 `docs/scaffold-guide.md` 文件，填写`.env.local`文件的内容，然后让claude检查数据库连接是否正常
  * 并把当前项目中的文件提交git版本管理(注意填写.gitignore，不要把.env和.env.local提交了)
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————