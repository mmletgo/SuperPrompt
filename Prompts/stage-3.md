### **第三阶段：界面布局与API定义 (Interface Layout & API Definition)**

#### **指令 1: 生成任务清单**

* **目标:** 为界面布局与API定义阶段创建详细的任务列表。  
* **智能体:** `sprint-prioritizer`  
* **任务:** 调用 `sprint-prioritizer`。**输入宏观目标：“设计界面线框图与API规范”。**`sprint-prioritizer` 必须读取 `designs/ux/user-flows.md`、`docs/front-end-spec.md`、`docs/fullstack-architecture.md`、`docs/task_format_spec.md`，然后输出两份独立的子任务清单：一份给`ux-researcher`（任务：创建线框图），另一份给`backend-architect`（任务：定义API规范）。生成的每个任务都必须严格按照 `docs/task_format_spec.md` 文件中定义的JSON结构进行创建，包含 `id`, `name`, `description`, `agent`, `status`, `dependencies`字段。。`sprint-prioritizer` 需读取 `Worknotes/stage-3-interface-layout.json` 和 `Worknotes/stage-3-api-definition.json` 的模板，将生成的任务列表分别填充到这两个文件的 `tasks` 数组中，然后将更新后的 JSON 对象写回原文件。**注意，`sprint-prioritizer`本阶段只需要规划： 界面线框图设计、API规范设计 两种任务,不要设计其它任何任务。**

#### **指令 2: 执行界面布局与API定义**

* **目标:** 根据已确认的任务清单，并行创建低保真线框图和详细的API规范。  
* **智能体:** `ux-researcher`, `backend-architect`  
* **任务:**  
  * **并行轨道 1: 线框图设计 (`ux-researcher`)**  
    * **操作：** 根据用户流程，创建低保真线框图。重点关注元素定位和功能，而非视觉样式。  
    * **输入：** 读取 `designs/ux/user-flows.md`、`docs/front-end-spec.md` 和 `Worknotes/stage-3-interface-layout.json` 文件。  
    * **输出：** 将线框图描述和简单布局保存到 `designs/ux/wireframes.md`，并更新 `Worknotes/stage-3-interface-layout.json` 中的任务状态为“completed”。  
  * **并行轨道 2: API规范定义 (`backend-architect`)**  
    * **操作：** 使用 OpenAPI 3.0 格式创建详细的 API 规范。定义前端运行所需的所有端点、请求负载和响应对象。  
    * **输入：** 读取 `docs/fullstack-architecture.md`（第 5 节）、`designs/ux/user-flows.md` 和 `Worknotes/stage-3-api-definition.json`。
    * **输出：** 将规范保存到 `docs/api-spec.md`，并更新 `Worknotes/stage-3-api-definition.json` 中的任务状态为“completed”。
* **交接文档更新:** 每个智能体在完成其任务后，必须读取其对应的 `Worknotes/stage-3-interface-layout.json` 或 `Worknotes/stage-3-api-definition.json` 文件，将相关任务的状态更新为“completed”，并在 `summary` 字段中记录工作摘要和产出文件位置，然后将更新后的 JSON 对象写回文件。