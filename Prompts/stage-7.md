### **第七阶段：前端集成 (Frontend Integration)**

#### **指令 1: 生成集成任务清单**

* **目标:** 创建一个详细的前端与后端API集成任务清单，并为每个任务生成一个独立的、包含所有必需上下文的Markdown文档。
* **智能体:** `sprint-prioritizer`
* **任务:**
  1. **分析与生成任务清单:** 调用 `sprint-prioritizer`，**输入宏观目标：“剔除掉之前的模拟API与前端的集成，将前端所有页面和组件与后端API进行集成”。** `sprint-prioritizer` 必须读取 `docs/api-spec.md`、`Worknotes/stage-5-frontend-development.json`、`Worknotes/stage-6-backend-development.json`、`docs/task_format_spec.md`，全面理解集成需求，并生成一个结构化的任务列表。
  2. **填充主JSON文件:** `sprint-prioritizer` 将生成的任务列表（此时 `task_document_path` 字段为空）填充到 `Worknotes/stage-7-integration.json` 文件的 `tasks` 数组中。**生成的每个任务都必须严格按照 `docs/task_format_spec.md` 文件中定义的JSON结构进行创建，包含 `id`, `name`, `description`, `agent`, `status`, `dependencies`字段。此清单必须按依赖关系排序，每个子任务的 `status`字段初始值必须为 `pending`。**
  3. **创建独立任务文档并更新路径:** 对于 `Worknotes/stage-7-integration.json` 中 `tasks` 数组的**每一个任务**，`sprint-prioritizer` 必须执行以下操作：
     * **创建独立Markdown文件:** 在 `Worknotes/tasks/stage7/` 目录下创建一个独立的 Markdown 文件，文件名应与任务名称(name)一致(例如: `integrate-user-login-api.md`)。
     * **填充文档内容:** 从 `docs/api-spec.md`、`Worknotes/stage-5-frontend-development.json` 和 `Worknotes/stage-6-backend-development.json` 中提取并整合与该任务**直接相关**的所有信息，写入新创建的 Markdown 文件中。内容应包括但不限于：
       * **相关API端点:** 请求/响应格式、URL、方法等。
       * **前端组件:** 涉及的页面、组件路径和功能说明。
       * **数据流:** 前后端交互的完整数据流和逻辑。
       * **测试要点:** 集成测试需要验证的关键功能点。
     * **更新主JSON文件:** `sprint-prioritizer` 必须将新创建的 Markdown 文件的相对路径（例如 `Worknotes/tasks/stage7/integrate-user-login-api.md`）更新到 `Worknotes/stage-7-integration.json` 文件中对应任务的 `task_document_path` 字段。这个过程是逐一完成的，确保每个任务都有一个链接到其详细上下文的文档。
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
#### **指令 2: 执行前端集成**

* **目标:** 将前端组件与实际的后端API连接起来，替换所有模拟数据，并确保端到端功能按预期工作。
* **智能体:** `frontend-developer`, `api-tester`, `backend-architect`
* **任务说明:** 采用动态工作流调度。你根据待处理任务的数量，在“单任务并行模式”和“多任务分批并行模式”之间自动切换。

* **工作流调度:**
  1. 你持续监控 `Worknotes/stage-7-integration.json`，筛选出所有状态为“pending”且（无依赖任务或所有依赖任务均已“completed”）的任务，注意，你要关注这个文件里每个任务**dependencies**这个字段，这是每个任务所依赖的任务id，你需要根据这个严格确定依赖关系。
  2. **如果符合条件的任务数量恰好为 1:** 启动 **“单任务并行模式”**。
  3. **如果符合条件的任务数量大于 1:** 启动 **“多任务分批并行模式”**。

* **模式一: 单任务并行模式 (Single-Task Parallel Mode)**
  * **调度:** 你针对该任务，同时调用 `frontend-developer` 和 `api-tester`，让它们并行执行，并将 `Worknotes/stage-7-integration.json` 中的任务状态更新为“in_progress”。
  * **并行执行:**
    * `frontend-developer`: 立即开始移除前端代码中的模拟数据，并调用真实的后端API。在执行任务时，如果发现系统中已存在与本次开发任务相关的代码，必须优先基于现有代码进行修改和完善，而不是从头开始重写。
    * `api-tester`: 立即开始编写端到端的集成测试脚本，并确保所有测试文件都存储在 `tests/` 目录下。在执行任务时，如果发现系统中已存在与本次开发任务相关的代码，必须优先基于现有代码进行修改和完善，而不是从头开始重写。
  * **开发-测试循环:**
    1. `frontend-developer` 完成初步集成后，通知 `api-tester`。
    2. `api-tester` 使用已编写的测试脚本进行端到端测试。
    3. **如果测试失败:** `api-tester` 将失败报告和日志保存到 `tests/reports/` 目录，然后将报告反馈给对应的开发人员。根据问题归属（前端问题由 `frontend-developer` 解决，后端问题由 `backend-architect` 解决），开发人员修复问题，然后重新进入上一步的测试环节。
    4. **如果测试通过:** `api-tester` 将 `Worknotes/stage-7-integration.json` 中的任务状态更新为“completed”。
  * **循环结束:** 任务完成后，流程返回 **“工作流调度”** 步骤。

* **模式二: 多任务分批并行模式 (Multi-Task Batch Mode)**
  * **步骤 1: 并行集成开发**
    * **调度:** 你为所有符合条件的任务并行调用 `frontend-developer`，并将 `Worknotes/stage-7-integration.json` 中的任务状态更新为“in_progress”。
    * **执行:** 每个 `frontend-developer` 根据任务需求完成前端与后端的集成。在执行任务时，如果发现系统中已存在与本次开发任务相关的代码，必须优先基于现有代码进行修改和完善，而不是从头开始重写。
    * **状态更新:** 开发完成后，`frontend-developer` 将 `Worknotes/stage-7-integration.json` 中对应任务的状态更新为“pending_test”。
  * **步骤 2: 并行测试**
    * **调度:** 你确认本批所有集成任务均已完成后，为所有“pending_test”的任务并行调用 `api-tester`。
    * **执行:** 每个 `api-tester` 编写并执行端到端集成测试，并确保所有测试文件都存储在 `tests/` 目录下。**在执行任务时，如果发现系统中已存在与本次开发任务相关的代码，必须优先基于现有代码进行修改和完善，而不是从头开始重写。**
    * **状态更新:**
      * **如果测试通过:** `api-tester` 将 `Worknotes/stage-7-integration.json` 中的任务状态更新为“completed”。
      * **如果测试失败:** `api-tester` 将 `Worknotes/stage-7-integration.json` 中的任务状态更新为“pending_fix”，并将失败日志保存到 `tests/reports/` 目录。
  * **步骤 3: 并行修复与回归测试**
    * **调度:** 你确认本批所有测试任务均已完成后，为所有“pending_fix”的任务并行调用对应的开发人员（`frontend-developer` 或 `backend-architect`）。
    * **执行:** 每个开发人员根据 `tests/reports/` 目录下的本任务对应的失败日志修复 Bug。
    * **状态更新:** 修复完成后，开发人员将 `Worknotes/stage-7-integration.json` 中的任务状态重新更新为“pending_test”。
    * **循环:** 流程将自动返回 **步骤 2: 并行测试**，形成一个“测试-修复”的循环，直到本批所有任务都变为“completed”，流程返回 **“工作流调度”** 步骤。

* **完成标准:**
  * `Worknotes/stage-7-integration.json` 中的所有任务状态均为 **“completed”**。
  * 整个应用程序功能完整，数据流在前后端之间正确传递，无功能性错误。
  * 所有端到端测试均已通过。

* **输入/输出规范:**
  * **输入:**
    1. **任务清单:** 所有智能体首先读取 `Worknotes/stage-7-integration.json` 文件来获取任务分配和状态。
    2. **任务专属文档:** 对于每个具体任务，`frontend-developer` 和 `api-tester` **必须**读取该任务JSON对象中 `task_document_path` 字段指向的独立Markdown文档，以获取开发和测试所需的全部上下文信息。
    3. **修复日志:** 在修复阶段，开发人员还需要读取 `tests/reports/` 目录下的相关失败日志。
  * **输出:** 更新后的前端代码提交至代码仓库。所有端到端测试脚本，测试用例均存储在 `tests/` 目录中。`Worknotes/stage-7-integration.json` 的任务状态被实时、准确地更新。测试失败时，报告和日志保存在 `tests/reports/` 目录。

* **完成度验证机制:** 为确保任务无遗漏，当所有任务完成后，你必须重新调用 `sprint-prioritizer`。`sprint-prioritizer` 的任务是最终审核 `Worknotes/stage-7-integration.json` 中的原始任务清单，确保所有任务都已被确认为“completed”。若发现未完成项，则必须将任务打回，并重新启动相应的集成/修复流程，直至所有任务被确认为100%完成。

* **交接文档更新:** 在所有任务都标记为“completed”后，你指派一个 `frontend-developer` 读取 `Worknotes/stage-7-integration.json`，在 `summary` 和 `next_stage_suggestions` 字段中记录工作摘要、端到端测试结果、以及对下一阶段（部署上线）的建议，然后将更新后的 JSON 对象写回文件，然后清理删除本阶段开发中生成的项目不需要的临时代码文件或日志文件(不要删除docs和Worknotes文件夹内部的文件)，接着把新增和修改的代码提交到git版本管理。

————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
## 用户操作
- 检查网页所有操作和逻辑是否符合你的预期，否则让frontend-developer和backend-architect继续修改直至符合你的预期再进行下一步
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
