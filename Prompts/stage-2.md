### **第二阶段：核心规划与数据建模 (Core Planning & Data Modeling)**

#### **指令 1: 生成任务清单**

* **目标:** 为核心规划与数据建模阶段创建详细的任务列表。  
* **智能体:** `sprint-prioritizer`  
* **任务:** 调用 `sprint-prioritizer`。**输入宏观目标：“创建所有用户流程图与数据库模型”。**`sprint-prioritizer` 必须读取 `docs/feature-scope.md` 、 `docs/prd.md`、`docs/fullstack-architecture.md`、`docs/task_format_spec.md`，然后输出两份独立的子任务清单：一份给`ux-researcher`（任务：创建用户流程），另一份给`backend-architect`（任务：定义数据库架构）。生成的每个任务都必须严格按照 `docs/task_format_spec.md` 文件中定义的JSON结构进行创建，包含 `id`, `name`, `description`, `agent`, `status`, `dependencies`, `outputs`, 和 `history` 字段。`sprint-prioritizer` 需读取 `Worknotes/stage-2-core-planning.json` 和 `Worknotes/stage-2-data-modeling.json` 的模板，将生成的任务列表分别填充到这两个文件的 `tasks` 数组中，然后将更新后的 JSON 对象写回原文件。**注意，`sprint-prioritizer`本阶段只需要规划：所有用户流程图的创建、数据库模型设计 两种任务,不要设计其它任何任务。**  

#### **指令 2: 执行核心规划与数据建模**

* **目标:** 根据已确认的任务清单，并行创建用户流程图和数据库基础架构。  
* **智能体:** `ux-researcher`, `backend-architect`  
* **任务:**  
  * **并行轨道 1: 用户流程设计 (`ux-researcher`)**  
    * **操作：** 使用 Mermaid 语法创建详细的用户流程图。这些流程必须涵盖`docs/feature-scope.md`中锁定的所有功能。  
    * **输入：** 读取 `docs/feature-scope.md`、`docs/prd.md` 和 `Worknotes/stage-2-core-planning.json` 文件。  
    * **输出：** 将图表保存到 `designs/ux/user-flows.md`，并更新 `Worknotes/stage-2-core-planning.json` 中的任务状态为“已完成”。
  * **并行轨道 2: 数据库架构 (`backend-architect`)**  
    * **操作：** 使用 `docs/fullstack-architecture.md` 中要求的ORM 语法定义完整的数据库架构。确保所有列、类型和关系均已正确定义，以支持 PRD 中的功能。  
    * **输入：** 读取 `docs/fullstack-architecture.md` （第 9 节）、`docs/prd.md` 和 `Worknotes/stage-2-data-modeling.json`。  
    * **输出：** 将完整的架构写入 `docs/schema.md`，并更新 `Worknotes/stage-2-data-modeling.json` 中的任务状态为“已完成”。  
* **交接文档更新:** 每个智能体在完成其任务后，必须读取其对应的 `Worknotes/stage-2-core-planning.json` 或 `Worknotes/stage-2-data-modeling.json` 文件，将相关任务的状态更新为“已完成”，并在 `summary` 字段中记录工作摘要和产出文件位置，然后将更新后的 JSON 对象写回文件。