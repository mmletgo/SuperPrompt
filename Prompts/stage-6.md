### **第六阶段：后端开发 (Backend Development)**

#### **指令 1: 生成任务清单**

* **目标:** 为后端开发创建详细的任务清单，并为每个任务生成一个独立的、包含所有必需上下文的Markdown文档。
* **智能体:** `sprint-prioritizer`
* **任务:**
  1. **分析与生成任务清单:** 调用 `sprint-prioritizer`，**输入宏观目标：“根据功能范围和前端需求，实现所有后端API”。**`sprint-prioritizer`必须读取`docs/prd.md`、`docs/fullstack-architecture.md`、`docs/feature-scope.md`、`docs/api-spec.md`、`docs/tech-stack.md`、`docs/schema.md`、`docs/task_format_spec.md`，全面理解集成需求，并生成一个结构化的、按资源和功能划分的API端点开发任务清单。
  2. **填充主JSON文件:** `sprint-prioritizer` 将生成的任务列表（此时 `task_document_path` 字段为空）填充到 `Worknotes/stage-6-backend-development.json` 文件的 `tasks` 数组中。**生成的每个任务都必须严格按照 `docs/task_format_spec.md` 文件中定义的JSON结构进行创建，包含 `id`, `name`, `description`, `agent`, `status`, `dependencies`字段。此清单必须按依赖关系排序，每个子任务的 `status`字段初始值必须为 `pending`。**
  3. **创建独立任务文档并更新路径:** 对于 `Worknotes/stage-6-backend-development.json` 中 `tasks` 数组的**每一个任务**，`sprint-prioritizer` 必须执行以下操作：
     * **创建独立Markdown文件:** 在 `Worknotes/tasks/stage6/` 目录下创建一个独立的 Markdown 文件，文件名应与任务名称(name)一致(例如: `create-user-api.md`)。
     * **填充文档内容:** 从所有相关源文档中提取并整合与该任务**直接相关**的所有信息，写入新创建的 Markdown 文件中。内容应包括详细的步骤、相关的API设计、数据模型、业务逻辑、以及任何特定的技术要求。
     * **更新主JSON文件:** `sprint-prioritizer` 必须将新创建的 Markdown 文件的相对路径更新到 `Worknotes/stage-6-backend-development.json` 文件中对应任务的 `task_document_path` 字段。这个过程是逐一完成的，确保每个任务都有一个链接到其详细上下文的文档。
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
#### **指令 2: 执行后端开发**

* **目标:** 通过动态工作流调度，高效、高质量地完成所有后端开发任务，并确保代码通过严格的测试验证。
* **智能体:** `backend-architect`, `api-tester`
* **任务说明:** 采用动态工作流调度。你根据待处理任务的数量，在“单任务并行模式”和“多任务分批并行模式”之间自动切换。

* **工作流调度:**
  1. 你持续监控 `Worknotes/stage-6-backend-development.json`，筛选出所有状态为“pending”且（无依赖任务或所有依赖任务均已“completed”）的任务，注意，你要关注这个文件里每个任务**依赖任务**这个字段，这是每个任务所依赖的任务id，你需要根据这个严格确定依赖关系。
  2. **如果符合条件的任务数量恰好为 1:** 启动 **“单任务并行模式”**。
  3. **如果符合条件的任务数量大于 1:** 启动 **“多任务分批并行模式”**。

* **模式一: 单任务并行模式 (Single-Task Parallel Mode)**
  * **调度:** 你针对该任务，同时调用 `backend-architect` 和 `api-tester`，让它们并行执行，并将 `Worknotes/stage-6-backend-development.json` 中的任务状态更新为“in_progress”。
  * **并行执行:**
    * `backend-architect`: 立即开始 API 端点和业务逻辑的开发。**在执行任务时，如果发现系统中已存在与本次开发任务相关的代码，必须优先基于现有代码进行修改和完善，而不是从头开始重写。**
    * `api-tester`: 立即开始编写测试脚本（如 Postman 集合、集成测试用例），并确保所有测试文件都存储在 `tests/` 目录下。**在执行任务时，如果发现系统中已存在与本次开发任务相关的代码，必须优先基于现有代码进行修改和完善，而不是从头开始重写。**
  * **开发-测试循环:**
    1. `backend-architect` 完成初步开发后，通知 `api-tester`。
    2. `api-tester` 使用已编写的测试脚本进行接口测试。
    3. **如果测试失败:** `api-tester` 将失败报告和日志保存到 `tests/reports/` 目录，然后将报告反馈给 `backend-architect`。`backend-architect` 修复问题，然后重新进入上一步的测试环节。
    4. **如果测试通过:** `api-tester` 将 `Worknotes/stage-6-backend-development.json` 中的任务状态更新为“completed”。
  * **循环结束:** 任务完成后，流程返回 **“工作流调度”** 步骤。

* **模式二: 多任务分批并行模式 (Multi-Task Batch Mode)**
  * **步骤 1: 并行功能开发**
    * **调度:** 你为所有符合条件的任务（无依赖任务或所有依赖任务均已“completed”）并行调用 `backend-architect`，并将 `Worknotes/stage-6-backend-development.json` 中的任务状态更新为“in_progress”。
    * **执行:** 每个 `backend-architect` 根据任务需求完成 API 开发，并确保所有测试文件都存储在 `tests/` 目录下。**在执行任务时，如果发现系统中已存在与本次开发任务相关的代码，必须优先基于现有代码进行修改和完善，而不是从头开始重写。**
    * **状态更新:** 开发完成后，`backend-architect` 将 `Worknotes/stage-6-backend-development.json` 中对应任务的状态更新为“pending_test”。
  * **步骤 2: 并行测试**
    * **调度:** 你确认本批所有开发任务均已完成后，为所有“pending_test”的任务并行调用 `api-tester`。
    * **执行:** 每个 `api-tester` 编写并执行接口测试。**在执行任务时，如果发现系统中已存在与本次开发任务相关的代码，必须优先基于现有代码进行修改和完善，而不是从头开始重写。**
    * **状态更新:**
      * **如果测试通过:** `api-tester` 将 `Worknotes/stage-6-backend-development.json` 中的任务状态更新为“completed”。
      * **如果测试失败:** `api-tester` 将 `Worknotes/stage-6-backend-development.json` 中的任务状态更新为“pending_fix”，并将失败日志保存到 `tests/reports/` 目录。
  * **步骤 3: 并行修复与回归测试**
    * **调度:** 你确认本批所有测试任务均已完成后，为所有“pending_fix”的任务并行调用 `backend-architect`。
    * **执行:** 每个 `backend-architect` 根据 `tests/reports/` 目录下的本任务对应的失败日志修复 Bug。
    * **状态更新:** 修复完成后，`backend-architect` 将 `Worknotes/stage-6-backend-development.json` 中的任务状态重新更新为“pending_test”。
    * **循环:** 流程将自动返回 **步骤 2: 并行测试**，形成一个“测试-修复”的循环，直到本批所有任务都变为“completed”，流程返回 **“工作流调度”** 步骤。

* **完成标准:**
  * `Worknotes/stage-6-backend-development.json` 中的所有任务状态均为 **“completed”**。
  * 所有 API 端点均通过 `api-tester` 的验证，符合 `docs/api-spec.md` 的规范。
  * 后端服务能够成功构建并运行，日志中无启动时错误。

* **输入/输出规范:**
  * **输入:** `backend-architect` 和 `api-tester` 首先读取 `Worknotes/stage-6-backend-development.json` 来获取分配给它们的任务。然后，对于每个任务，它们必须读取该任务的 `task_document_path` 字段所指向的独立 Markdown 文档来获取所有开发和测试所需的上下文信息。在修复阶段，`backend-architect` 还需要读取 `tests/reports/` 目录下的任务对应的日志。
  * **输出:** 所有开发完成的后端代码提交至代码仓库。所有测试用例均存储在 `tests/` 目录中。`Worknotes/stage-6-backend-development.json` 的任务状态被实时、准确地更新。测试失败时，报告和日志保存在 `tests/reports/` 目录。

* **完成度验证机制:** 为确保任务无遗漏，当所有任务完成后，你必须重新调用 `sprint-prioritizer`。`sprint-prioritizer` 的任务是最终审核 `Worknotes/stage-6-backend-development.json` 中的原始任务清单，确保所有任务都已被确认为“completed”。若发现未完成项，则必须将任务打回，并重新启动相应的开发/修复流程，直至所有任务被确认为100%完成。

* **交接文档更新:** 在所有任务都标记为“completed”后，你指派一个 `backend-architect` 读取 `Worknotes/stage-6-backend-development.json`，在 `summary` 和 `next_stage_suggestions` 字段中记录工作摘要、API 部署和运行说明、以及对下一阶段（前端集成）的建议，然后将更新后的 JSON 对象写回文件，然后清理删除本阶段开发中生成的项目不需要的临时代码文件或日志文件(不要删除docs和Worknotes文件夹内部的文件)，接着把新增和修改的代码提交到git版本管理。

————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
## 用户操作
- 检查每个API是否都能正常调用，返回信息是否符合你的预期，否则让backend-architect继续修改直至符合你的预期再进行下一步
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
