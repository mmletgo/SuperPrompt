### **第四阶段：UI设计 (UI Design)**

#### **指令 1: 生成任务清单**

* **目标:** 为UI设计阶段创建详细的、按优先级排序的组件和页面设计任务。  
* **智能体:** `sprint-prioritizer`  
* **任务:** 调用 `sprint-prioritizer`。**输入宏观目标：“根据UX线框图，创建高保真UI设计系统和所有核心界面的视觉稿html”。**`sprint-prioritizer` 必须读取 `designs/ux/wireframes.md`、`designs/ux/user-flows.md`、`docs/front-end-spec.md`和`docs/task_format_spec.md`，然后输出一份详细的、按优先级排序的组件和页面设计UI子任务清单。**生成的每个任务都必须严格按照 `docs/task_format_spec.md` 文件中定义的JSON结构进行创建，包含 `id`, `name`, `description`, `agent`, `status`, `dependencies`, `outputs`, 和 `history` 字段。。**此清单将被填充到 `Worknotes/stage-4-ui-design.json` 文件的 `tasks` 数组中，然后将更新后的 JSON 对象写回文件。

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
    1. `ui-designer` 严格按照`Worknotes/stage-4-ui-design.json`清单顺序执行，读取 `designs/ux/` 中的文件，使用 shadcn/ui 和 Tailwind CSS 的设计理念，创建高保真UI设计稿html和UI设计规范。  
    2. 完成后，`whimsy-injector` 读取 `ui-designer` 的设计稿html，为其注入微交互和动画效果，特别是针对状态变更、层级导航动画和AI自动更新等场景。  
* **输入:** `ui-designer` 读取 `designs/ux/` 和 `Worknotes/stage-4-ui-design.json`。`whimsy-injector` 读取 `ui-designer` 的产出和 `Worknotes/stage-4-ui-design.json`。
  * **输出:**  
    * `ui-designer` 必须将高保真UI设计稿保存到 `designs/ui/` 目录，并将UI设计规范保存到 `designs/ui/design-spec.md`。  
    * `whimsy-injector` 将包含微交互定义的最终设计稿更新到 `designs/ui/` 目录。  
    * 所有智能体在完成对应子任务后，都需读取 `Worknotes/stage-4-ui-design.json`，将对应子任务的状态更新为“已完成”，然后将更新后的 JSON 对象写回文件。  
* **交接文档更新:** `whimsy-injector` 完成任务后，必须读取 `Worknotes/stage-4-ui-design.json`，在 `summary` 和 `next_stage_suggestions` 字段中记录工作摘要、产出文件位置和对下一阶段的建议，然后将更新后的 JSON 对象写回文件。