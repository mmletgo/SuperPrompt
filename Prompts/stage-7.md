### **第七阶段：部署与上线 (Integration, Deployment & Launch)**

#### **指令 1: 生成上线任务清单**

* **目标:** 创建一份详细的、按优先级排序的上线任务清单，并为每个任务生成一个独立的、包含所有必需上下文的Markdown文档。
* **智能体:** `sprint-prioritizer`
* **任务:**
  1. **分析与生成任务清单:** 调用 `sprint-prioritizer`，**输入宏观目标：“清理项目中除了docs和Worknotes文件夹之外所有开发过程中生成的临时文件和临时代码。完成应用的端到端测试，自动化部署并正式上线”。** `sprint-prioritizer` 必须读取 `docs/api-spec.md`、`Worknotes/stage-5-backend-development.json`、`Worknotes/stage-6-frontend-development.json`、`docs/tech-stack.md`、`docs/fullstack-architecture.md` 和 `docs/task_format_spec.md`，全面理解上线需求，并生成一个结构化的任务列表。**任务生成必须严格按照负责的智能体（agent）进行分类，确保每个任务都明确分配给 `api-tester`, `devops-automator`, `infrastructure-maintainer`, 或 `project-shipper` 中的一个。**
  2. **填充主JSON文件:** `sprint-prioritizer` 将生成的任务列表（此时 `task_document_path` 字段为空）填充到 `Worknotes/stage-7-integration-deployment-and-launch.json` 文件的 `tasks` 数组中。**生成的每个任务都必须严格按照 `docs/task_format_spec.md` 文件中定义的JSON结构进行创建，包含 `id`, `name`, `description`, `agent`, `status`, `dependencies`字段。此清单必须按依赖关系和优先级排序，每个子任务的 `status`字段初始值必须为 `pending`。**
  3. **创建独立任务文档并更新路径:** 对于 `Worknotes/stage-7-integration-deployment-and-launch.json` 中 `tasks` 数组的**每一个任务**，`sprint-prioritizer` 必须执行以下操作：
     * **创建独立Markdown文件:** 在 `Worknotes/tasks/stage8/` 目录下创建一个独立的 Markdown 文件，文件名应清晰地反映任务内容（例如 `setup-ci-cd-pipeline.md`）。
     * **填充文档内容:** 从所有相关源文档中提取并整合与该任务**直接相关**的所有信息，写入新创建的 Markdown 文件中。内容应包括详细的步骤、配置信息、相关脚本和预期的输出。
     * **更新主JSON文件:** `sprint-prioritizer` 必须将新创建的 Markdown 文件的相对路径更新到 `Worknotes/stage-7-integration-deployment-and-launch.json` 文件中对应任务的 `task_document_path` 字段。这个过程是逐一完成的，确保每个任务都有一个链接到其详细上下文的文档。
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
#### **指令 2: 执行部署与上线**

* **目标:** 自动化地执行 `Worknotes/stage-7-integration-deployment-and-launch.json` 文件中定义的上线任务，将应用部署到生产环境，建立监控，并正式发布。
* **主控智能体:** `studio-coach`
* **协作智能体:** `api-tester`, `devops-automator`, `infrastructure-maintainer`, `project-shipper`
* **工作流:**
  1. **任务监控与调度:** `studio-coach` 作为主控，持续监控 `Worknotes/stage-7-integration-deployment-and-launch.json` 文件。
  2. **筛选可执行任务:** `studio-coach` 筛选出所有状态为 `pending` 且其 `dependencies` 数组中的所有任务ID都已标记为 `completed` 的任务。
  3. **任务分派:** 对于每一个筛选出的可执行任务，`studio-coach` 执行以下操作：
     * **更新状态:** 将任务状态更新为 `in_progress`。
     * **调用相应智能体:** 根据任务对象中的 `agent` 字段，调用对应的智能体（例如 `api-tester`, `devops-automator` 等）。
     * **传递任务上下文:** `studio-coach` 必须向被调用的智能体明确指示，其唯一的任务上下文来源是该任务 `task_document_path` 字段指向的Markdown文件。智能体必须严格按照该文档中的指示执行任务。
  4. **任务执行与状态反馈:**
     * 被调用的智能体在完成任务后，必须将 `Worknotes/stage-7-integration-deployment-and-launch.json` 中对应任务的状态更新为 `completed`。
     * 如果在执行过程中遇到无法解决的错误，智能体应将状态更新为 `failed`，并在 `history` 字段中记录详细的错误信息。`studio-coach` 需要对此类失败任务进行处理，安排对应的agent重新处理。
     * **代码复用原则:** 所有智能体在执行任务时，如果发现系统中已存在与本次开发任务相关的代码（如配置文件、脚本等），必须优先基于现有代码进行修改和完善，而不是从头开始重写。
  5. **循环与完成:** `studio-coach` 不断重复步骤2-4，直到 `Worknotes/stage-7-integration-deployment-and-launch.json` 中的所有任务状态都变为 `completed`。

* **完成标准:**
  * `Worknotes/stage-7-integration-deployment-and-launch.json` 中的所有任务状态均为 **“completed”**。
  * CI/CD管道能够自动化地将应用成功部署到生产环境。
  * `project-shipper` 在执行其最终上线任务时，确认应用在生产URL上功能正常，核心用户流程无误。
* **交接文档更新:** `project-shipper` 在其负责的最后一个任务（通常是正式发布）完成后，必须读取 `Worknotes/stage-7-integration-deployment-and-launch.json`，在 `summary` 字段中记录最终的生产应用URL和项目成功上线的最终声明，然后将更新后的 JSON 对象写回文件，然后清理删除本阶段开发中生成的项目不需要的临时代码文件或日志文件(不要删除docs和Worknotes文件夹内部的文件)，接着把新增和修改的代码提交到git版本管理。