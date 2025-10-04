### **第六阶段：前端开发 (Frontend Development)**

#### **指令 1: 生成前端开发任务清单**

* **目标:** 为前端开发阶段创建详细的、按依赖关系和优先级排序的前端开发任务清单。
* **注意事项: 项目已经初始化Next.js，开发环境已配置好，认证系统已实现，shadcn-ui也已经初始化完成，`docs/api-spec.md`中描述的后端API端点都已实现。**
* **智能体:** `sprint-prioritizer`
* **任务:**
  1. **分析与生成任务清单:** 调用 `sprint-prioritizer`，**输入宏观目标："严格按照designs/ui/设计稿实现应用的所有前端功能，确保界面效果与设计稿完全一致"。** `sprint-prioritizer` 必须读取 `designs/ux/`、`designs/ui/`、`designs/ui/design-spec.md`、`docs/feature-scope.md`、`docs/api-spec.md`、`docs/tech-stack.md`、`docs/task_format_spec.md`，全面理解需求，并生成一个结构化的、按依赖关系和优先级排序的任务列表。**特别注意：所有UI组件和界面必须严格遵循designs/ui/中的设计稿，不能仅使用shadcn-ui的原始组件样式，必须在此基础上添加自定义样式、动效和视觉效果以匹配设计稿。** 任务清单需按以下任务类别顺序组织：
     * **基础环境搭建:** 项目初始化、开发环境配置、依赖安装等
     * **基础组件开发:** 按设计规范实现可复用的UI基础组件
     * **核心组件开发:** 实现业务相关的复杂组件
     * **页面布局开发:** 实现应用程序的整体布局和导航结构
     * **页面功能开发:** 实现各个页面的具体功能和交互
     * **对接后端开发:** 实现各个组件页面与后端api的集成
  2. **填充主JSON文件:** `sprint-prioritizer` 将生成的任务列表填充到 `Worknotes/stage-6-frontend-development.json` 文件的 `tasks` 数组中。**生成的每个任务都必须严格按照 `docs/task_format_spec.md` 文件中定义的JSON结构进行创建，包含 `id`, `name`, `description`, `agent`, `status`, `dependencies`字段。此清单必须按依赖关系排序，每个子任务的 `status`字段初始值必须为 `pending`。**
———————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
#### **指令 2: 执行前端开发**

* **目标:** 基于高保真设计稿，通过动态工作流调度，高效、高质量地完成所有前端开发任务，并确保代码通过严格的测试验证。**关键要求：所有界面实现必须与designs/ui/目录中的设计稿完全一致，包括颜色、字体、间距、动画效果等所有视觉细节。严禁仅使用shadcn-ui组件的默认样式，必须基于设计稿进行深度定制。**
* **智能体:** `frontend-developer`, `test-writer-fixer`
* **任务说明:** 采用动态工作流调度。你根据待处理任务的数量，在“单任务并行模式”和“多任务分批并行模式”之间自动切换,**直到完成所有任务**。
* **输入/输出规范:**
  * **输入:** `frontend-developer` 和 `test-writer-fixer` 首先读取 `Worknotes/stage-6-frontend-development.json` 来获取分配给它们的任务。然后统一读取核心设计和技术文档：`designs/ux/`、`designs/ui/`、`designs/ui/design-spec.md`、`docs/feature-scope.md`、`docs/api-spec.md`、`docs/tech-stack.md`、`docs/task_format_spec.md` 来获取所有开发和测试所需的上下文信息。在修复阶段，`frontend-developer` 还需要读取 `tests/reports/` 目录下的任务对应的日志。
  * **输出:** 所有开发完成的前端代码提交至代码仓库。所有测试用例均存储在 `tests/` 目录中。模拟API代码保存在 `api/mock/` 目录。测试失败时，报告和日志保存在 `tests/reports/` 目录。`Worknotes/stage-6-frontend-development.json` 的任务状态被实时、准确地更新。
* **完成标准:**
  * `Worknotes/stage-6-frontend-development.json` 中的所有任务状态均为 **"completed"**。
  * 项目能够通过 `npm run dev` 成功启动，并且在浏览器中访问所有页面和核心功能时，开发者工具的控制台中没有任何错误。
  * **UI界面与designs/ui/设计稿完全一致，包括所有视觉细节和交互效果**
  * 所有 UI 效果、布局、微动画均需符合 `designs/` 目录下的设计规范，并通过 `chrome-devtools` mcp 测试验证。
* **工作流调度:**
  1. 你持续监控 `Worknotes/stage-6-frontend-development.json`，筛选出所有状态为“pending”且（无依赖任务或所有依赖任务均已“completed”）的任务，注意，你要关注这个文件里每个任务**dependencies**这个字段，这是每个任务所依赖的任务id，你需要根据这个严格确定依赖关系。
  2. **如果符合条件的任务数量恰好为 1:** 启动 **“单任务并行模式”**。
  3. **如果符合条件的任务数量大于 1:** 启动 **“多任务分批并行模式”**，上限为5个一组。

* **模式一: 单任务并行模式 (Single-Task Parallel Mode)**
  * **调度:** 你针对该任务，同时调用 `frontend-developer` 和 `test-writer-fixer`，让它们并行执行，并将 `Worknotes/stage-6-frontend-development.json` 中的任务状态更新为“in_progress”。
  * **并行执行:**
    * `frontend-developer`: 立即开始功能开发。**在执行任务时，如果发现系统中已存在与本次开发任务相关的代码，必须优先基于现有代码进行修改和完善，而不是从头开始重写。开发过程中必须严格对照designs/ui/中的设计稿，确保每个组件的视觉效果、交互动画、颜色搭配等完全匹配设计稿要求**
    * `test-writer-fixer`: 立即开始编写测试用例，并确保所有测试文件都存储在 `tests/` 目录下。**在执行任务时，如果发现系统中已存在与本次开发任务相关的代码，必须优先基于现有代码进行修改和完善，而不是从头开始重写。测试用例应包括UI视觉效果验证，确保实现的界面与设计稿一致。**
  * **开发-测试循环:**
    1. `frontend-developer` 完成初步开发后，通知已准备好的 `test-writer-fixer`。
    2. `test-writer-fixer` 使用已编写的测试用例进行测试。
    3. **如果测试失败:** `test-writer-fixer` 将失败报告和日志保存到 `tests/reports/` 目录，然后将报告反馈给 `frontend-developer`。`frontend-developer` 修复问题，然后重新进入上一步的测试环节。
    4. **如果测试通过:** `test-writer-fixer` 将 `Worknotes/stage-6-frontend-development.json` 中的任务状态更新为“completed”。
  * **循环结束:** 任务完成后，流程返回 **“工作流调度”** 步骤。


* **模式二: 多任务分批并行模式 (Multi-Task Batch Mode)**
  * **步骤 1: 并行功能开发**
    * **调度:** 你为所有符合条件的任务（无依赖任务或所有依赖任务均已“completed”）并行调用 `frontend-developer`，并将 `Worknotes/stage-6-frontend-development.json` 中的任务状态更新为“in_progress”。
    * **执行:** 每个 `frontend-developer` 根据任务需求完成功能开发。**在执行任务时，如果发现系统中已存在与本次开发任务相关的代码，必须优先基于现有代码进行修改和完善，而不是从头开始重写。开发过程中必须严格参照designs/ui/设计稿，实现精确的视觉效果，包括但不限于：颜色值、字体大小、组件间距、圆角半径、阴影效果、动画过渡等所有设计细节。**
    * **状态更新:** 开发完成后，`frontend-developer` 将 `Worknotes/stage-6-frontend-development.json` 中对应任务的状态更新为“pending_test”。
  * **步骤 2: 并行测试**
    * **调度:** 你确认本批所有开发任务均已完成后，为所有“pending_test”的任务并行调用 `test-writer-fixer`。
    * **执行:** 每个 `test-writer-fixer` 编写并执行测试用例。所有新编写的测试用例文件都必须存储在 `tests/` 目录下。**在执行任务时，如果发现系统中已存在与本次开发任务相关的代码，必须优先基于现有代码进行修改和完善，而不是从头开始重写。测试用例必须包含UI视觉效果验证，通过截图对比或样式检查等方式确保实现的界面与designs/ui/设计稿完全一致。**
    * **状态更新:**
      * **如果测试通过:** `test-writer-fixer` 将 `Worknotes/stage-6-frontend-development.json` 中的任务状态更新为“completed”。
      * **如果测试失败:** `test-writer-fixer` 将 `Worknotes/stage-6-frontend-development.json` 中的任务状态更新为“pending_fix”，并将失败日志保存到 `tests/reports/` 目录。
  * **步骤 3: 并行修复与回归测试**
    * **调度:** 你确认本批所有测试任务均已完成后，为所有“pending_fix”的任务并行调用 `frontend-developer`。
    * **执行:** 每个 `frontend-developer` 根据 `tests/reports/` 目录下的本任务对应的失败日志修复 Bug。
    * **状态更新:** 修复完成后，`frontend-developer` 将 `Worknotes/stage-6-frontend-development.json` 中的任务状态重新更新为“pending_test”。
    * **循环:** 流程将自动返回 **步骤 2: 并行测试**，形成一个“测试-修复”的循环，直到本批所有任务都变为“completed”，流程返回 **“工作流调度”** 步骤。

* **完成度验证机制:** 为确保任务无遗漏，当所有任务完成后，你必须重新调用 `sprint-prioritizer`。`sprint-prioritizer` 的任务是最终审核 `Worknotes/stage-6-frontend-development.json` 中的原始任务清单，确保所有任务都已被确认为"completed"。若发现未完成项，则必须将任务打回，并重新启动相应的开发/修复流程，直至所有任务被确认为100%完成。

* **交接文档更新:** 在所有任务都标记为"completed"后，你指派一个 `frontend-developer` 读取 `Worknotes/stage-6-frontend-development.json`，在 `summary` 和 `next_stage_suggestions` 字段中记录工作摘要、项目部署和运行说明、以及对下一阶段（集成测试或生产部署）的建议，然后将更新后的 JSON 对象写回文件，然后清理删除本阶段开发中生成的项目不需要的临时代码文件或日志文件(不要删除docs和Worknotes文件夹内部的文件)，接着把新增和修改的代码提交到git版本管理。
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
## 用户操作
- 注册一个测试账号给下一步指令使用
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
#### **指令 3: 前端视觉一致性验收**
## 角色定位
你是一个项目测试调度协调员，负责协调测试-修复循环流程，不直接执行测试工作。

## 当前状态
- 项目运行在：http://localhost:3000
- 问题记录文件：`docs/qa.md`
- 可用资源：`test-writer-fixer`(负责测试)，`frontend-developer`(负责修复)

## 调度流程

### 阶段1：启动测试
召唤`test-writer-fixer`执行以下测试任务(**禁止创建测试页面，而是直接使用正式页面测试**)：
1. 使用chrome-devtools和测试账号(邮箱: test2@example.com 密码: superTest111!)依次测试所有页面(**流程完全按照`designs/ux/user-flows.md`**),**页面ui要完全按照`designs/ui/`中的html视觉稿实现(每个html对应一个页面)**
2. 你需要按页面清单创建todo list，然后按todo list逐页面测试，确保页面与视觉稿一致。
3. 将所有发现的问题详细记录在`docs/qa.md`文件中，**不要生成乱码**

### 阶段2：检查结果
检查`docs/qa.md`文件状态：
- 如果文件为空：结束流程，向项目测试调度协调员(主agent)报告所有测试通过
- 如果文件中有问题记录：进入修复阶段

### 阶段3：启动修复
召唤`frontend-developer`执行以下修复任务：
1. 解决`docs/qa.md`文件中记录的所有问题
2. 召唤`test-writer-fixer`验证修复结果，如果未成功修复，则打回去让`frontend-developer`继续修复，直到当前`docs/qa.md`文件中提到的问题彻底被解决
3. 修复完成后**清空`docs/qa.md`文件的内容**

### 阶段4：循环控制
重复执行阶段1→阶段2→阶段3，直到阶段2检查发现`docs/qa.md`文件为空

## 调度规则
- 始终通过召唤`test-writer-fixer`和`frontend-developer`来执行具体工作
- 每次只给`test-writer-fixer`和`frontend-developer`一个明确的任务（测试、修复）
- 根据任务结果决定下一步调度
- 保持循环直到连续两轮测试无新问题发现(即**连续两轮的阶段2都检查`docs/qa.md`发现没有问题。是没发现问题！不是发现问题然后解决掉了**)
- 即使阶段2检测到严重问题，你也不应该停下来，而是继续阶段3,让`frontend-developer`执行修复任务


#### **指令 4: 前端验收**

## 角色定位
你是一个项目测试调度协调员，负责协调测试-修复循环流程，不直接执行测试工作。

## 当前状态
- 项目运行在：http://localhost:3000
- 问题记录文件：`docs/qa.md`
- 可用资源：`test-writer-fixer`(负责测试)，`frontend-developer`(负责修复)

## 调度流程

### 阶段1：启动测试
召唤`test-writer-fixer`执行以下测试任务(**禁止创建测试页面，而是直接使用正式页面测试**)：
1. `test-writer-fixer`使用chrome-devtools和测试账号(邮箱: test2@example.com 密码: superTest111!)全面测试所有页面、交互跳转和功能(**流程完全按照`designs/ux/user-flows.md`**，界面结构和交互要完全按照`designs/ux/wireframes/`中的各个页面和组件的md文件)
2. `test-writer-fixer`需要按页面清单创建todo list，然后按todo list逐页面测试，确保页面交互与线框图一致。
3. `test-writer-fixer`将所有发现的问题详细记录在`docs/qa.md`文件中，**不要生成乱码**

### 阶段2：检查结果
你检查`docs/qa.md`文件状态：
- 如果文件为空：结束流程，报告所有测试通过
- 如果文件中有问题记录：进入修复阶段

### 阶段3：启动修复
召唤`frontend-developer`执行以下修复任务：
1. `frontend-developer`解决`docs/qa.md`文件中记录的所有问题
2. 召唤`test-writer-fixer`验证修复结果，如果未成功修复，则打回去让`frontend-developer`继续修复，直到当前`docs/qa.md`文件中提到的问题彻底被解决
3. 修复完成后`test-writer-fixer`**清空`docs/qa.md`文件的内容**

### 阶段4：循环控制
重复执行阶段1→阶段2→阶段3，直到阶段2检查发现`docs/qa.md`文件为空

## 调度规则
- 始终通过召唤`test-writer-fixer`和`frontend-developer`来执行具体工作
- 每次只给`test-writer-fixer`和`frontend-developer`一个明确的任务（测试、修复）
- 根据任务结果决定下一步调度
- 保持循环直到连续两轮测试无新问题发现(即**连续两轮的阶段2都检查`docs/qa.md`发现没有问题。是没发现问题！不是发现问题然后解决掉了**)
- 即使阶段2检测到严重问题，你也不应该停下来，而是继续阶段3,让`frontend-developer`执行修复任务


————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
## 用户操作
- 检查前端界面和操作是否符合你的预期，否则让frontend-developer继续修改直至符合你的预期再进行下一步
- 指令3可以多次运行(注意用/clear清空上下文后再运行，这样相当于每次有不同的测试，注意清空qa.md，llm不一定会清空它，需要你手动处理)
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
