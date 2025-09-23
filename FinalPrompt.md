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

     # 创建工作记录目录，并为每个阶段生成独立的记录文件(空文件)  
     mkdir -p Worknotes  
     touch Worknotes/stage-1-planning-and-initialization.md  
     touch Worknotes/stage-2-core-planning.md
     touch Worknotes/stage-2-data-modeling.md
     touch Worknotes/stage-3-interface-layout-and-api-definition.md  
     touch Worknotes/stage-4-ui-design.md  
     touch Worknotes/stage-5-frontend-development.md  
     touch Worknotes/stage-6-backend-development.md
     touch Worknotes/stage-7-integration.md
     touch Worknotes/stage-8-integration-deployment-and-launch.md

     echo "协作目录结构创建完成。"
  4. **提醒用户:**
     * **暂停并明确提示用户:** "项目依赖已安装。现在，请您打开 `do-j-pm/.env.local` 文件，并根据 `docs/scaffold-guide.md` 的指导，填入您自己的数据库、Supabase和API密钥等敏感信息。**这是项目运行的关键步骤。** "

#### 用户执行
  * 阅读 `docs/scaffold-guide.md` 文件，填写`.env.local`文件的内容


### **第二阶段：核心规划与数据建模 (Core Planning & Data Modeling)**

#### **指令 1: 生成任务清单**

* **目标:** 为核心规划与数据建模阶段创建详细的任务列表。  
* **智能体:** `sprint-prioritizer`  
* **任务:** 调用 `sprint-prioritizer`。**输入宏观目标：“创建所有用户流程图与数据库模型”。**`sprint-prioritizer` 必须读取 `docs/feature-scope.md` 、 `docs/prd.md` 和 `docs/fullstack-architecture.md`，然后输出两份独立的子任务清单：一份给`ux-researcher`（任务：创建用户流程），另一份给`backend-architect`（任务：定义数据库架构），分别将任务清单完整记录在 `Worknotes/stage-2-core-planning.md`和`Worknotes/stage-2-data-modeling.md` 中，**注意，`sprint-prioritizer`本阶段只需要规划：所有用户流程图的创建、数据库模型设计 两种任务,不要设计其它任何任务。**  

#### **指令 2: 执行核心规划与数据建模**

* **目标:** 根据已确认的任务清单，并行创建用户流程图和数据库基础架构。  
* **智能体:** `ux-researcher`, `backend-architect`  
* **任务:**  
  * **并行轨道 1: 用户流程设计 (`ux-researcher`)**  
    * **操作：** 使用 Mermaid 语法创建详细的用户流程图。这些流程必须涵盖`docs/feature-scope.md`中锁定的所有功能。  
    * **输入：** 读取 `docs/feature-scope.md`、`docs/prd.md` 和 `Worknotes/stage-2-core-planning.md` 文件。  
    * **输出：** 将图表保存到 `designs/ux/user-flows.md`，并更新 `Worknotes/stage-2-core-planning.md` 中的任务状态为“已完成”。
  * **并行轨道 2: 数据库架构 (`backend-architect`)**  
    * **操作：** 使用 `docs/fullstack-architecture.md` 中要求的ORM 语法定义完整的数据库架构。确保所有列、类型和关系均已正确定义，以支持 PRD 中的功能。  
    * **输入：** 读取 `docs/fullstack-architecture.md` （第 9 节）、`docs/prd.md` 和 `Worknotes/stage-2-data-modeling.md`。  
    * **输出：** 将完整的架构写入 `docs/schema.md`，并更新 `Worknotes/stage-2-data-modeling.md` 中的任务状态为“已完成”。  
* **交接文档更新:** 每个智能体在完成其任务后，必须在 `Worknotes/stage-2-core-planning.md` 或 `Worknotes/stage-2-data-modeling.md` 中将对应的子任务标记为“已完成”，并简要说明产出文件的位置。

### **第三阶段：界面布局与API定义 (Interface Layout & API Definition)**

#### **指令 1: 生成任务清单**

* **目标:** 为界面布局与API定义阶段创建详细的任务列表。  
* **智能体:** `sprint-prioritizer`  
* **任务:** 调用 `sprint-prioritizer`。**输入宏观目标：“设计界面线框图与API规范”。**`sprint-prioritizer` 必须读取 `designs/ux/user-flows.md`、`docs/front-end-spec.md` 和 `docs/fullstack-architecture.md`，然后输出两份独立的子任务清单：一份给`ux-researcher`（任务：创建线框图），另一份给`backend-architect`（任务：定义API规范），分别将任务清单完整记录在 `Worknotes/stage-3-interface-layout.md`和`Worknotes/stage-3-api-definition.md` 中，**注意，`sprint-prioritizer`本阶段只需要规划： 界面线框图设计、API规范设计 两种任务,不要设计其它任何任务。**

#### **指令 2: 执行界面布局与API定义**

* **目标:** 根据已确认的任务清单，并行创建低保真线框图和详细的API规范。  
* **智能体:** `ux-researcher`, `backend-architect`  
* **任务:**  
  * **并行轨道 1: 线框图设计 (`ux-researcher`)**  
    * **操作：** 根据用户流程，创建低保真线框图。重点关注元素定位和功能，而非视觉样式。  
    * **输入：** 读取 `designs/ux/user-flows.md`、`docs/front-end-spec.md` 和 `Worknotes/stage-3-interface-layout.md` 文件。  
    * **输出：** 将线框图描述和简单布局保存到 `designs/ux/wireframes.md`，并更新 `Worknotes/stage-3-interface-layout.md` 中的任务状态为“已完成”。  
  * **并行轨道 2: API规范定义 (`backend-architect`)**  
    * **操作：** 使用 OpenAPI 3.0 格式创建详细的 API 规范。定义前端运行所需的所有端点、请求负载和响应对象。  
    * **输入：** 读取 `docs/fullstack-architecture.md`（第 5 节）、`designs/ux/user-flows.md` 和 `Worknotes/stage-3-api-definition.md`。
    * **输出：** 将规范保存到 `docs/api-spec.md`，并更新 `Worknotes/stage-3-api-definition.md` 中的任务状态为“已完成”。
* **交接文档更新:** `ux-researcher` 和 `backend-architect` 在完成各自的所有任务后，必须在 `Worknotes/stage-3-interface-layout.md` 和 `Worknotes/stage-3-api-definition.md` 文件中追加各自的工作摘要、产出文件位置和对下一阶段的建议。

### **第四阶段：UI设计 (UI Design)**

#### **指令 1: 生成任务清单**

* **目标:** 为UI设计阶段创建详细的、按优先级排序的组件和页面设计任务。  
* **智能体:** `sprint-prioritizer`  
* **任务:** 调用 `sprint-prioritizer`。**输入宏观目标：“根据UX线框图，创建高保真UI设计系统和所有核心界面的视觉稿html”。**`sprint-prioritizer` 必须读取 `designs/ux/wireframes.md`、`designs/ux/user-flows.md` 和 `docs/front-end-spec.md`，然后输出一份详细的、按优先级排序的组件和页面设计UI子任务清单，记录在 `Worknotes/stage-4-ui-design.md` 中。  

#### **指令 2: 执行UI设计**

* **目标:** 将已批准的线框图和用户流程转化为高保真、可投入生产的设计系统，并注入微交互和动画效果。  
* **智能体:** `ui-designer`, `whimsy-injector`  
* **任务分配 (分层构建):**  
  * 任务说明:  
    UI设计规范: `ui-designer` 需要创建详细的设计规范文档。它必须包含：  
    * 调色板：主色、中性色和状态色，并包含十六进制代码。  
    * 排版：所有文本元素的字体选择、大小和粗细。  
    * 组件设计：对于`docs/front-end-spec.md`中标识的每个关键组件，提供视觉描述，列出其不同状态（默认、悬停、活动、禁用、屏蔽），并指定填充/边距。  
    * 微交互：`whimsy-injector` 负责描述关键动画，例如向下钻取过渡和自动状态更改。  
  * **任务执行:**  
    1. `ui-designer` 严格按照`Worknotes/stage-4-ui-design.md`清单顺序执行，读取 `designs/ux/` 中的文件，使用 shadcn/ui 和 Tailwind CSS 的设计理念，创建高保真UI设计稿html和UI设计规范。  
    2. 完成后，`whimsy-injector` 读取 `ui-designer` 的设计稿html，为其注入微交互和动画效果，特别是针对状态变更、层级导航动画和AI自动更新等场景。  
* **输入:** `ui-designer` 读取 `designs/ux/` 和 `Worknotes/stage-4-ui-design.md`。`whimsy-injector` 读取 `ui-designer` 的产出和 `Worknotes/stage-4-ui-design.md`。
  * **输出:**  
    * `ui-designer` 必须将高保真UI设计稿保存到 `designs/ui/` 目录，并将UI设计规范保存到 `designs/ui/design-spec.md`。  
    * `whimsy-injector` 将包含微交互定义的最终设计稿更新到 `designs/ui/` 目录。  
    * 所有智能体在完成对应子任务后，都需将 `Worknotes/stage-4-ui-design.md` 中对应的子任务状态更新为已完成状态。  
* **交接文档更新:** `whimsy-injector` 完成任务后，必须在 `Worknotes/stage-4-ui-design.md` 文件中追加工作摘要、产出文件位置和对下一阶段的建议。

### **第五阶段：前端开发 (Frontend Development)**

#### **指令 1: 生成任务清单**

* **目标:** 创建一个按组件和页面划分的、优先级明确的前端开发任务清单。  
* **智能体:** `sprint-prioritizer`  
* **任务:** 调用 `sprint-prioritizer`。**输入宏观目标：“实现应用的所有前端功能”。**`sprint-prioritizer` 必须读取`designs/ux/`、`designs/ui/`、`designs/ui/design-spec.md`、`docs/feature-scope.md`、`docs/api-spec.md`、`docs/tech-stack.md`，然后输出一份前端开发子任务清单，记录在 `Worknotes/stage-5-frontend-development.md` 中。**此清单必须按依赖关系排序，没有依赖的任务排在最前面。每个任务都应明确列出其依赖的任务编号。每个子任务都应有”完成状态“字段，初始是”未开始“**子任务清单需按以下任务类别顺序组织：1. 组件开发；2. 根据 `docs/api-spec.md` 创建模拟 API；3. 应用程序布局和页面开发；4. 将 UI 连接到模拟 API。  

#### **指令 2: 执行前端开发**

* **目标:** 基于高保真设计稿，通过动态工作流调度，高效、高质量地完成所有前端开发任务，并确保代码通过严格的测试验证。
* **智能体:** `frontend-developer`, `test-writer-fixer`
* **任务说明:** 采用动态工作流调度。你根据待处理任务的数量，在“单任务并行模式”和“多任务分批并行模式”之间自动切换。

* **工作流调度:**
  1. 你持续监控 `Worknotes/stage-5-frontend-development.md`，筛选出所有状态为“未开始”且（无依赖任务或所有依赖任务均已“已完成”）的任务，注意，你要关注这个文件里每个任务**依赖任务**这个字段，这是每个任务所依赖的任务id，你需要根据这个严格确定依赖关系。
  2. **如果符合条件的任务数量恰好为 1:** 启动 **“单任务并行模式”**。
  3. **如果符合条件的任务数量大于 1:** 启动 **“多任务分批并行模式”**。

* **模式一: 单任务并行模式 (Single-Task Parallel Mode)**
  * **调度:** 你针对该任务，同时调用 `frontend-developer` 和 `test-writer-fixer`，并将 `Worknotes/stage-5-frontend-development.md` 中的任务状态更新为“进行中”。
  * **并行执行:**
    * `frontend-developer`: 立即开始功能开发。
    * `test-writer-fixer`: 立即开始编写测试用例，并确保所有测试文件都存储在 `tests/` 目录下。
  * **开发-测试循环:**
    1. `frontend-developer` 完成初步开发后，通知已准备好的 `test-writer-fixer`。
    2. `test-writer-fixer` 使用已编写的测试用例进行测试。
    3. **如果测试失败:** `test-writer-fixer` 将失败报告和日志保存到 `tests/reports/` 目录，然后将报告反馈给 `frontend-developer`。`frontend-developer` 修复问题，然后重新进入上一步的测试环节。
    4. **如果测试通过:** `test-writer-fixer` 将 `Worknotes/stage-5-frontend-development.md` 中的任务状态更新为“已完成”。
  * **循环结束:** 任务完成后，流程返回 **“工作流调度”** 步骤。

* **模式二: 多任务分批并行模式 (Multi-Task Batch Mode)**
  * **步骤 1: 并行功能开发**
    * **调度:** 你为所有符合条件的任务（无依赖任务或所有依赖任务均已“已完成”）并行调用 `frontend-developer`，并将 `Worknotes/stage-5-frontend-development.md` 中的任务状态更新为“进行中”。
    * **执行:** 每个 `frontend-developer` 根据任务需求完成功能开发。
    * **状态更新:** 开发完成后，`frontend-developer` 将 `Worknotes/stage-5-frontend-development.md` 中对应任务的状态更新为“待测试”。
  * **步骤 2: 并行测试**
    * **调度:** 你确认本批所有开发任务均已完成后，为所有“待测试”的任务并行调用 `test-writer-fixer`。
    * **执行:** 每个 `test-writer-fixer` 编写并执行测试用例。所有新编写的测试用例文件都必须存储在 `tests/` 目录下。
    * **状态更新:**
      * **如果测试通过:** `test-writer-fixer` 将 `Worknotes/stage-5-frontend-development.md` 中的任务状态更新为“已完成”。
      * **如果测试失败:** `test-writer-fixer` 将 `Worknotes/stage-5-frontend-development.md` 中的任务状态更新为“待修复”，并将失败日志保存到 `tests/reports/` 目录。
  * **步骤 3: 并行修复与回归测试**
    * **调度:** 你确认本批所有测试任务均已完成后，为所有“待修复”的任务并行调用 `frontend-developer`。
    * **执行:** 每个 `frontend-developer` 根据 `tests/reports/` 目录下的本任务对应的失败日志修复 Bug。
    * **状态更新:** 修复完成后，`frontend-developer` 将 `Worknotes/stage-5-frontend-development.md` 中的任务状态重新更新为“待测试”。
    * **循环:** 流程将自动返回 **步骤 2: 并行测试**，形成一个“测试-修复”的循环，直到本批所有任务都变为“已完成”，流程返回 **“工作流调度”** 步骤。

* **完成标准:**
  * `Worknotes/stage-5-frontend-development.md` 中的所有任务状态均为 **“已完成”**。
  * 项目能够通过 `npm run dev` 成功启动，并且在浏览器中访问所有页面和核心功能时，开发者工具的控制台中没有任何错误。
  * 所有 UI 效果、布局、微动画均需符合 `designs/` 目录下的设计规范，并通过 `playwright` mcp 测试验证。

* **输入/输出规范:**
  * **输入:** `frontend-developer` 和 `test-writer-fixer` 读取 `designs/ux/`、`designs/ui/`、`designs/ui/design-spec.md`、`docs/api-spec.md` 和 `Worknotes/stage-5-frontend-development.md`。在修复阶段，`frontend-developer` 还需要读取 `tests/reports/` 目录下的任务对应的日志。
  * **输出:** 所有开发完成的前端代码提交至代码仓库。所有测试用例均存储在 `tests/` 目录中。测试失败时，报告和日志保存在 `tests/reports/` 目录。`Worknotes/stage-5-frontend-development.md` 的任务状态被实时、准确地更新。

* **交接文档更新:** 在所有任务都标记为“已完成”后，你指派一个 `frontend-developer` 在 `Worknotes/stage-5-frontend-development.md` 中追加最终的工作摘要、对下一阶段的建议和项目运行说明。

* **完成度验证机制:** 为确保任务无遗漏，当所有任务完成后，你必须重新调用 `sprint-prioritizer`。`sprint-prioritizer` 的任务是最终审核 `Worknotes/stage-5-frontend-development.md` 中的原始任务清单，确保所有任务都已被确认为“已完成”。若发现未完成项，则必须将任务打回，并重新启动相应的开发/修复流程，直至所有任务被确认为100%完成。

### **第六阶段：后端开发 (Backend Development)**

#### **指令 1: 生成任务清单**

* **目标:** 为后端开发创建详细的任务清单。
* **智能体:** `sprint-prioritizer`
* **任务:** 调用 `sprint-prioritizer`。
  * **输入宏观目标：“根据功能范围和前端需求，实现所有后端API”。**`sprint-prioritizer`必须读取 `docs/prd.md`、`docs/fullstack-architecture.md`、`docs/feature-scope.md`、`docs/api-spec.md`、`docs/tech-stack.md`、`docs/schema.md` 和 `Worknotes/stage-5-frontend-development.md`，输出一份详细的、按资源和功能划分的API端点开发任务清单，记录在`Worknotes/stage-6-backend-development.md`中。**此清单必须按依赖关系排序，没有依赖的任务排在最前面。每个任务都应明确列出其依赖的任务编号。**
* **用户确认点:** AI在此暂停，等待用户审核并确认 `Worknotes/stage-6-backend-development.md` 中的任务清单。

#### **指令 2: 执行后端开发**

* **目标:** 通过动态工作流调度，高效、高质量地完成所有后端开发任务，并确保代码通过严格的测试验证。
* **智能体:** `backend-developer`, `api-tester`
* **任务说明:** 采用动态工作流调度。你根据待处理任务的数量，在“单任务并行模式”和“多任务分批并行模式”之间自动切换。

* **工作流调度:**
  1. 你持续监控 `Worknotes/stage-6-backend-development.md`，筛选出所有状态为“未开始”且（无依赖任务或所有依赖任务均已“已完成”）的任务，注意，你要关注这个文件里每个任务**依赖任务**这个字段，这是每个任务所依赖的任务id，你需要根据这个严格确定依赖关系。
  2. **如果符合条件的任务数量恰好为 1:** 启动 **“单任务并行模式”**。
  3. **如果符合条件的任务数量大于 1:** 启动 **“多任务分批并行模式”**。

* **模式一: 单任务并行模式 (Single-Task Parallel Mode)**
  * **调度:** 你针对该任务，同时调用 `backend-developer` 和 `api-tester`，并将 `Worknotes/stage-6-backend-development.md` 中的任务状态更新为“进行中”。
  * **并行执行:**
    * `backend-developer`: 立即开始 API 端点和业务逻辑的开发。
    * `api-tester`: 立即开始编写测试脚本（如 Postman 集合、集成测试用例）。
  * **开发-测试循环:**
    1. `backend-developer` 完成初步开发后，通知 `api-tester`。
    2. `api-tester` 使用已编写的测试脚本进行接口测试。
    3. **如果测试失败:** `api-tester` 将失败报告和日志保存到 `tests/reports/` 目录，然后将报告反馈给 `backend-developer`。`backend-developer` 修复问题，然后重新进入上一步的测试环节。
    4. **如果测试通过:** `api-tester` 将 `Worknotes/stage-6-backend-development.md` 中的任务状态更新为“已完成”。
  * **循环结束:** 任务完成后，流程返回 **“工作流调度”** 步骤。

* **模式二: 多任务分批并行模式 (Multi-Task Batch Mode)**
  * **步骤 1: 并行功能开发**
    * **调度:** 你为所有符合条件的任务（无依赖任务或所有依赖任务均已“已完成”）并行调用 `backend-developer`，并将 `Worknotes/stage-6-backend-development.md` 中的任务状态更新为“进行中”。
    * **执行:** 每个 `backend-developer` 根据任务需求完成 API 开发。
    * **状态更新:** 开发完成后，`backend-developer` 将 `Worknotes/stage-6-backend-development.md` 中对应任务的状态更新为“待测试”。
  * **步骤 2: 并行测试**
    * **调度:** 你确认本批所有开发任务均已完成后，为所有“待测试”的任务并行调用 `api-tester`。
    * **执行:** 每个 `api-tester` 编写并执行接口测试。
    * **状态更新:**
      * **如果测试通过:** `api-tester` 将 `Worknotes/stage-6-backend-development.md` 中的任务状态更新为“已完成”。
      * **如果测试失败:** `api-tester` 将 `Worknotes/stage-6-backend-development.md` 中的任务状态更新为“待修复”，并将失败日志保存到 `tests/reports/` 目录。
  * **步骤 3: 并行修复与回归测试**
    * **调度:** 你确认本批所有测试任务均已完成后，为所有“待修复”的任务并行调用 `backend-developer`。
    * **执行:** 每个 `backend-developer` 根据 `tests/reports/` 目录下的本任务对应的失败日志修复 Bug。
    * **状态更新:** 修复完成后，`backend-developer` 将 `Worknotes/stage-6-backend-development.md` 中的任务状态重新更新为“待测试”。
    * **循环:** 流程将自动返回 **步骤 2: 并行测试**，形成一个“测试-修复”的循环，直到本批所有任务都变为“已完成”，流程返回 **“工作流调度”** 步骤。

* **完成标准:**
  * `Worknotes/stage-6-backend-development.md` 中的所有任务状态均为 **“已完成”**。
  * 所有 API 端点均通过 `api-tester` 的验证，符合 `docs/api-spec.md` 的规范。
  * 后端服务能够成功构建并运行，日志中无启动时错误。

* **输入/输出规范:**
  * **输入:** `backend-developer` 和 `api-tester` 读取 `docs/api-spec.md`、`docs/schema.md` 和 `Worknotes/stage-6-backend-development.md`。在修复阶段，`backend-developer` 还需要读取 `tests/reports/` 目录下的任务对应的日志。
  * **输出:** 所有开发完成的后端代码提交至代码仓库。API 测试脚本或集合。`Worknotes/stage-6-backend-development.md` 的任务状态被实时、准确地更新。测试失败时，报告和日志保存在 `tests/reports/` 目录。

* **完成度验证机制:** 为确保任务无遗漏，当所有任务完成后，你必须重新调用 `sprint-prioritizer`。`sprint-prioritizer` 的任务是最终审核 `Worknotes/stage-6-backend-development.md` 中的原始任务清单，确保所有任务都已被确认为“已完成”。若发现未完成项，则必须将任务打回，并重新启动相应的开发/修复流程，直至所有任务被确认为100%完成。

* **交接文档更新:** 在所有任务都标记为“已完成”后，你指派一个 `backend-developer` 在 `Worknotes/stage-6-backend-development.md` 中追加最终的工作摘要、API 部署和运行说明、以及对下一阶段（前端集成）的建议。

### **第七阶段：前端集成 (Frontend Integration)**

#### **指令 1: 生成任务清单**

* **目标:** 为前端与后端API的集成创建详细的任务清单。
* **智能体:** `sprint-prioritizer`
* **任务:** 调用 `sprint-prioritizer`。
  * **输入宏观目标：“将前端与实时API集成”。**`sprint-prioritizer` 需要分析前端代码结构、`docs/api-spec.md`、`Worknotes/stage-5-frontend-development.md`、`Worknotes/stage-6-backend-development.md` 和 `docs/fullstack-architecture.md`，输出一份按页面或组件划分的、详细的集成子任务清单，记录在`Worknotes/stage-7-integration.md`中。**此清单同样必须按依赖关系排序，并明确列出依赖关系。**
* **用户确认点:** AI在此暂停，等待用户审核并确认 `Worknotes/stage-7-integration.md` 中的任务清单。

#### **指令 2: 执行前端集成**

* **目标:** 将前端应用与真实的后端API集成。
* **智能体:** `frontend-developer`
* **任务执行 (前端集成):** `frontend-developer` 严格按照清单顺序执行以下任务：
    * **输入:** `frontend-developer` 需读取 `Worknotes/stage-7-integration.md` 中的集成任务清单，并参考 `docs/api-spec.md` 和 `Worknotes/stage-6-backend-development.md` 进行开发。
  * **任务：将前端与实时 API 集成**
    * **操作：** 移除模拟 API 客户端。更新所有前端组件和页面，以便向新创建的后端 API 发出实时请求。为所有服务器交互的加载、错误和成功状态实施适当的状态管理。
* **完成度验证机制:** 为确保前端任务无遗漏，当 `frontend-developer` 宣称完成所有集成任务后，必须重新调用 `sprint-prioritizer`。`sprint-prioritizer` 的任务是对照 `Worknotes/stage-7-integration.md` 中的集成任务清单进行最终审核。若发现未完成的任务，则必须将任务打回，并要求 `frontend-developer` 继续工作，直至所有任务被确认为100%完成。
* **交接文档更新:**
  * `frontend-developer` 在完成集成任务后，必须更新 `Worknotes/stage-7-integration.md` 并添加“前后端集成完成”的最终声明。

### **第八阶段：联调、部署与上线 (Integration, Deployment & Launch)**

#### **指令 1: 生成任务清单**

* **目标:** 创建一份详细的、按优先级排序的上线任务清单。
* **智能体:** `sprint-prioritizer`
* **任务:** 调用 `sprint-prioritizer`。**输入宏观目标：“完成应用的端到端测试，自动化部署并正式上线”。**`sprint-prioritizer` 必须读取 `docs/api-spec.md`、`Worknotes/stage-6-backend-development.md`、`Worknotes/stage-7-integration.md`、`docs/tech-stack.md` 和 `docs/fullstack-architecture.md`，然后输出一份详细的、按优先级排序的子任务清单，记录在 `Worknotes/stage-8-integration-deployment-and-launch.md` 中，例如：
  1. 执行完整的端到端（E2E）集成测试。
  2. 使用基础设施即代码（IaC）配置生产环境。
  3. 构建持续集成/持续部署（CI/CD）管道。
  4. 部署到预生产（Staging）环境并进行最终验收。
  5. 执行向生产环境的正式部署。
  6. 设置生产环境的监控、日志和警报系统。
  7. 完成上线检查清单并正式发布。
* **用户确认点:** AI在此暂停，等待用户审核并确认 `Worknotes/stage-8-integration-deployment-and-launch.md` 中的任务清单。

#### **指令 2: 执行部署与上线**

* **目标:** 将应用部署到生产环境，建立监控，并正式发布。
* **智能体:** `studio-coach`, `api-tester`, `devops-automator`, `infrastructure-maintainer`, `project-shipper`
* **智能体都要输入:** `Worknotes/stage-8-integration-deployment-and-launch.md`
* **任务执行 (协作流程):**
  1. **端到端测试 (E2E Testing):**
     * 指令 **`api-tester`**：“读取 `docs/api-spec.md`、`Worknotes/stage-6-backend-development.md` 和 `Worknotes/stage-7-integration.md`。针对已完成集成的应用，执行一次完整的端到端测试，模拟真实用户的核心操作流（如注册-\>登录-\>核心功能-\>退出）。验证从UI到数据库的完整链路。将详尽的测试报告保存到 `testing/e2e-report.md`。”
  2. **构建自动化部署管道 (CI/CD Pipeline):**
     * 指令 **`devops-automator`**：“分析项目的`docs/tech-stack.md`技术栈。创建自动化CI/CD管道（例如，使用GitHub Actions），该管道必须能自动完成测试、构建和部署流程。将CI/CD配置文件（如 .github/workflows/deploy.yml）保存到代码库。”
  3. **生产环境准备与监控 (Production Setup):**
     * 指令 **`infrastructure-maintainer`**：“根据 `docs/fullstack-architecture.md`和`docs/tech-stack.md`，配置生产环境。设置实时的性能监控、日志聚合和关键错误警报系统。确保应用具备弹性伸缩能力以应对流量高峰。”
  4. **正式上线 (Go-Live):**
     * 指令 **`project-shipper`**：“这是最终的上线决策。请执行发布前检查清单，确认 `testing/e2e-report.md` 测试通过、CI/CD管道运行成功、监控系统已激活。确认无误后，触发生产部署流程，将应用正式发布。”
* **完成标准:**
  * 所有端到端测试100%通过。
  * CI/CD管道能够自动化地将应用成功部署到生产环境。
  * `project-shipper` 确认应用在生产URL上功能正常，核心用户流程无误。
* **交接文档更新:** `project-shipper` 在应用成功上线后，必须在 `Worknotes/stage-8-integration-deployment-and-launch.md` 的最顶部添加最终的生产应用URL，并附上项目成功上线的最终声明。