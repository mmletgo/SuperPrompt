### **第四阶段：UI设计 (UI Design)**

#### **指令 1: 生成UI设计任务清单**

* **目标:** 为UI设计阶段创建详细的、按依赖关系和优先级排序的UI设计任务清单，专注于`ui-designer`的工作范围。
* **智能体:** `sprint-prioritizer`
* **任务:**
  1. **分析与生成UI设计任务清单:** 调用 `sprint-prioritizer`，**输入宏观目标："根据UX线框图，创建高保真UI设计系统md文档和界面视觉稿html"。** `sprint-prioritizer` 必须读取 `designs/ux/wireframes.md`、`designs/ux/user-flows.md`、`docs/front-end-spec.md`、`docs/task_format_spec.md`，全面理解需求，并生成一个结构化的、按依赖关系和优先级排序的UI设计任务列表。任务清单需按以下任务类别顺序组织：1. 设计系统基础（调色板、字体、基础组件规范）；2. 核心UI组件设计；3. 页面布局和界面设计。
  2. **填充主JSON文件:** `sprint-prioritizer` 将生成的UI设计任务列表填充到 `Worknotes/stage-4-ui-design.json` 文件的 `tasks` 数组中。**生成的每个任务都必须严格按照 `docs/task_format_spec.md` 文件中定义的JSON结构进行创建，包含 `id`, `name`, `description`, `agent`（设置为"ui-designer"）, `status`, `dependencies`字段。此清单必须按依赖关系排序，每个子任务的 `status`字段初始值必须为 `pending`。**
——————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
#### **指令 2: 执行UI设计任务**

* **目标:** 基于已批准的线框图和用户流程，高效、高质量地完成所有UI设计任务，创建高保真、可投入生产的设计系统和界面视觉稿。
* **智能体:** `ui-designer`
* **任务说明:** 采用动态工作流调度。你根据待处理任务的数量，在"单任务模式"和"多任务并行模式"之间自动切换。

* **工作流调度:**
  1. 你持续监控 `Worknotes/stage-4-ui-design.json`，筛选出所有状态为"pending"且agent为"ui-designer"且（无依赖任务或所有依赖任务均已"completed"）的任务，注意，你要关注这个文件里每个任务**依赖任务**这个字段，这是每个任务所依赖的任务id，你需要根据这个严格确定依赖关系。
  2. **如果符合条件的任务数量恰好为 1:** 启动 **"单任务模式"**。
  3. **如果符合条件的任务数量大于 1:** 启动 **"多任务并行模式"**。

* **模式一: 单任务模式 (Single-Task Mode)**
  * **调度:** 你针对该任务调用 `ui-designer`，并将 `Worknotes/stage-4-ui-design.json` 中的任务状态更新为"in_progress"。
  * **执行:** `ui-designer` 立即开始UI设计工作，读取 `designs/ux/` 中的文件和依赖任务的产出，使用 shadcn/ui 和 Tailwind CSS 的设计理念，创建高保真UI设计稿html和UI设计规范。**在执行任务时，如果发现系统中已存在与本次设计任务相关的文件，必须优先基于现有文件进行修改和完善，而不是从头开始重写。**
  * **状态更新:** 设计完成后，`ui-designer` 将 `Worknotes/stage-4-ui-design.json` 中对应任务的状态更新为"completed"。
  * **循环结束:** 任务完成后，流程返回 **"工作流调度"** 步骤。

* **模式二: 多任务并行模式 (Multi-Task Parallel Mode)**
  * **调度:** 你为所有符合条件的任务（无依赖任务或所有依赖任务均已"completed"）并行调用 `ui-designer`，并将 `Worknotes/stage-4-ui-design.json` 中的任务状态更新为"in_progress"。
  * **执行:** 每个 `ui-designer` 根据任务需求和依赖任务的产出完成UI设计工作。**在执行任务时，如果发现系统中已存在与本次设计任务相关的文件，必须优先基于现有文件进行修改和完善，而不是从头开始重写。**
  * **状态更新:** 设计完成后，`ui-designer` 将 `Worknotes/stage-4-ui-design.json` 中对应任务的状态更新为"completed"。
  * **循环:** 流程将自动返回 **"工作流调度"** 步骤。

* **完成标准:**
  * `Worknotes/stage-4-ui-design.json` 中所有agent为"ui-designer"的任务状态均为 **"completed"**。
  * 所有UI设计稿html文件已保存到 `designs/ui/` 目录。
  * UI设计规范已保存到 `designs/ui/design-spec.md`。

* **输入/输出规范:**
  * **输入:** `ui-designer` 首先读取 `Worknotes/stage-4-ui-design.json` 来获取分配给它的任务。然后读取 `designs/ux/` 中的相关文件，以及依赖任务在 `designs/ui/` 目录中的产出文件作为输入。
  * **输出:** `ui-designer` 必须将**高保真UI设计稿html保存到 `designs/ui/` 目录**，并将**UI设计规范保存到 `designs/ui/design-spec.md`**。`Worknotes/stage-4-ui-design.json` 的任务状态被实时、准确地更新。

* **完成度验证机制:** 为确保任务无遗漏，当所有UI设计任务完成后，你必须重新调用 `sprint-prioritizer`。`sprint-prioritizer` 的任务是最终审核 `Worknotes/stage-4-ui-design.json` 中agent为"ui-designer"的任务清单，确保所有任务都已被确认为"completed"。若发现未完成项，则必须将任务打回，并重新启动相应的UI设计流程，直至所有任务被确认为100%完成。

————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
## 用户操作
- 检查UI是否符合你的预期，否则让ui-designer继续修改直至符合你的预期再进行下一步
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————

#### **指令 3: 生成微交互增强任务清单**

* **目标:** 基于已完成的UI设计稿，为微交互和动画效果注入创建详细的、按依赖关系和优先级排序的任务清单，专注于`whimsy-injector`的工作范围。
* **智能体:** `sprint-prioritizer`
* **任务:**
  1. **分析与生成微交互任务清单:** 调用 `sprint-prioritizer`，**输入宏观目标："基于已完成的UI设计稿，为界面注入微交互和动画效果"。** `sprint-prioritizer` 必须读取 `designs/ui/` 目录中的所有UI设计稿、`designs/ux/user-flows.md`、`docs/front-end-spec.md`、`docs/task_format_spec.md`，全面理解需求，并生成一个结构化的、按依赖关系和优先级排序的微交互增强任务列表。任务清单需按以下任务类别顺序组织：1. 基础组件微交互（按钮、表单、导航等）；2. 页面级交互动画（页面切换、状态变更）；3. 高级交互效果（AI自动更新、数据可视化动画）。
  2. **更新主JSON文件:** `sprint-prioritizer` 将生成的微交互任务列表追加到 `Worknotes/stage-4-ui-whimsy.json` 文件的 `tasks` 数组中。**生成的每个任务都必须严格按照 `docs/task_format_spec.md` 文件中定义的JSON结构进行创建，包含 `id`, `name`, `description`, `agent`（设置为"whimsy-injector"）, `status`, `dependencies`字段。微交互任务的依赖关系应该包含对应的UI设计任务ID。此清单必须按依赖关系排序，每个子任务的 `status`字段初始值必须为 `pending`。**
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
#### **指令 4: 执行微交互增强任务**

* **目标:** 基于已完成的UI设计稿，高效、高质量地为所有界面注入微交互和动画效果，提升用户体验。
* **智能体:** `whimsy-injector`
* **任务说明:** 采用动态工作流调度。你根据待处理任务的数量，在"单任务模式"和"多任务并行模式"之间自动切换。

* **工作流调度:**
  1. 你持续监控 `Worknotes/stage-4-ui-whimsy.json`，筛选出所有状态为"pending"且agent为"whimsy-injector"且（无依赖任务或所有依赖任务均已"completed"）的任务，注意，你要关注这个文件里每个任务**依赖任务**这个字段，这是每个任务所依赖的任务id，你需要根据这个严格确定依赖关系。
  2. **如果符合条件的任务数量恰好为 1:** 启动 **"单任务模式"**。
  3. **如果符合条件的任务数量大于 1:** 启动 **"多任务并行模式"**。

* **模式一: 单任务模式 (Single-Task Mode)**
  * **调度:** 你针对该任务调用 `whimsy-injector`，并将 `Worknotes/stage-4-ui-whimsy.json` 中的任务状态更新为"in_progress"。
  * **执行:** `whimsy-injector` 立即开始微交互增强工作，读取对应的UI设计稿和依赖任务的产出，为其注入微交互和动画效果，特别是针对状态变更、层级导航动画和AI自动更新等场景。**在执行任务时，如果发现系统中已存在与本次设计任务相关的文件，必须优先基于现有文件进行修改和完善，而不是从头开始重写。**
  * **状态更新:** 增强完成后，`whimsy-injector` 将 `Worknotes/stage-4-ui-whimsy.json` 中对应任务的状态更新为"completed"。
  * **循环结束:** 任务完成后，流程返回 **"工作流调度"** 步骤。

* **模式二: 多任务并行模式 (Multi-Task Parallel Mode)**
  * **调度:** 你为所有符合条件的任务（无依赖任务或所有依赖任务均已"completed"）并行调用 `whimsy-injector`，并将 `Worknotes/stage-4-ui-whimsy.json` 中的任务状态更新为"in_progress"。
  * **执行:** 每个 `whimsy-injector` 读取对应的UI设计稿和依赖任务的产出，为其注入微交互和动画效果。**在执行任务时，如果发现系统中已存在与本次设计任务相关的文件，必须优先基于现有文件进行修改和完善，而不是从头开始重写。**
  * **状态更新:** 增强完成后，`whimsy-injector` 将 `Worknotes/stage-4-ui-whimsy.json` 中对应任务的状态更新为"completed"。
  * **循环:** 流程将自动返回 **"工作流调度"** 步骤。

* **完成标准:**
  * `Worknotes/stage-4-ui-whimsy.json` 中所有agent为"whimsy-injector"的任务状态均为 **"completed"**。
  * 所有包含微交互定义的最终设计稿已更新到 `designs/ui/` 目录。
  * 所有 UI 效果、布局、微动画均符合设计要求和用户体验标准。

* **输入/输出规范:**
  * **输入:** `whimsy-injector` 首先读取 `Worknotes/stage-4-ui-whimsy.json` 来获取分配给它的任务。然后读取 `designs/ui/` 中的相关UI设计稿文件，以及依赖任务的产出文件作为输入。
  * **输出:** `whimsy-injector` 将包含微交互定义的最终设计稿更新到 `designs/ui/` 目录。`Worknotes/stage-4-ui-whimsy.json` 的任务状态被实时、准确地更新。

* **完成度验证机制:** 为确保任务无遗漏，当所有微交互增强任务完成后，你必须重新调用 `sprint-prioritizer`。`sprint-prioritizer` 的任务是最终审核 `Worknotes/stage-4-ui-whimsy.json` 中agent为"whimsy-injector"的任务清单，确保所有任务都已被确认为"completed"。若发现未完成项，则必须将任务打回，并重新启动相应的微交互增强流程，直至所有任务被确认为100%完成。

* **交接文档更新:** 在所有任务都标记为"completed"后，你指派一个 `whimsy-injector` 读取 `Worknotes/stage-4-ui-whimsy.json`，在 `summary` 和 `next_stage_suggestions` 字段中记录工作摘要、产出文件位置和对下一阶段的建议，然后将更新后的 JSON 对象写回文件。

————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
## 用户操作
- 检查交互动画是否符合你的预期，否则让whimsy-injector继续修改直至符合你的预期再进行下一步
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
