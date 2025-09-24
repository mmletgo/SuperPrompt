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

#### 用户执行
**执行项目初始化:**
  * 阅读 `docs/scaffold-guide.md` 文件，确认项目初始化所需的命令和配置，并执行命令，比如npm create t3-app@latest do-j-pm。
  * **交互式配置指导:** 由于此命令是交互式的，需要根据 `docs/scaffold-guide.md` 中的“配置选项”部分进行选择。

**指令2: 初始化项目并配置环境 (Initialize Project & Configure Environment):**

* **目标:** 根据 `docs/scaffold-guide.md` 初始化项目，并引导用户完成关键配置。
* **智能体:** `project-manager`
* **任务:**
  1. **安装依赖与环境配置:**
     * **进入项目目录:** 所有后续命令都必须在 `do-j-pm` 目录下执行。
     * **安装依赖:** 根据 `docs/scaffold-guide.md` 的“项目依赖安装”部分，生成并执行所有 `npm install` 命令。
     * **创建环境文件:** 修改.env.example以包含系统所需要的所有字段，然后执行 `copy .env.example .env.local`，。
  2. **设计文件结构 (Design File Structure):**
     * 基于 `docs/fullstack-architecture.md` 中定义的结构，在项目初始化并安装依赖后，设计一个详细的目录和文件结构。
     * 此结构必须包含用于存放各阶段记录的 Worknotes 文件夹，并将设计稿、文档、Prompt等内容明确分区。
     * 将设计的目录树结构保存到 `docs/project-structure.md`。
  3. **应用文件结构并创建协作目录 (Apply File Structure & Create Collab Dirs):**
     * **检查目录:** 检查 `do-j-pm` 目录是否已成功创建。
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
     echo {"tasks": [], "summary": "", "next_stage_suggestions": ""} > Worknotes/stage-5-frontend-development.json
     echo {"tasks": [], "summary": "", "next_stage_suggestions": ""} > Worknotes/stage-6-backend-development.json
     echo {"tasks": [], "summary": "", "next_stage_suggestions": ""} > Worknotes/stage-7-integration.json
     echo {"tasks": [], "summary": "", "next_stage_suggestions": ""} > Worknotes/stage-8-integration-deployment-and-launch.json

     echo "协作目录结构与工作记录JSON文件创建完成。"
  4. **定义任务JSON结构 (Define Task JSON Structure):**
     * 为了确保整个项目中任务管理的一致性和自动化，所有在 `tasks` 数组中创建的任务都必须遵循 `docs/task_format_spec.md` 文件中定义的JSON结构。此文件定义了任务的核心属性，包括依赖关系、产出物和历史记录，是后续所有阶段任务创建和状态跟踪的唯一标准。
  5. **提醒用户:**
     * **暂停并明确提示用户:** "项目依赖已安装。现在，请您打开 `do-j-pm/.env.local` 文件，并根据 `docs/scaffold-guide.md` 的指导，填入您自己的数据库、Supabase和API密钥等敏感信息。**这是项目运行的关键步骤。** "

#### 用户执行
  * 阅读 `docs/scaffold-guide.md` 文件，填写`.env.local`文件的内容