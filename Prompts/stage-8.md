### **第八阶段：联调、部署与上线 (Integration, Deployment & Launch)**

#### **指令 1: 生成上线任务清单**

* **目标:** 创建一份详细的、按优先级排序的上线任务清单，并为每个任务生成一个独立的、包含所有必需上下文的Markdown文档。
* **智能体:** `sprint-prioritizer`
* **任务:**
  1. **分析与生成任务清单:** 调用 `sprint-prioritizer`，**输入宏观目标：“完成应用的端到端测试，自动化部署并正式上线”。** `sprint-prioritizer` 必须读取 `docs/api-spec.md`、`Worknotes/stage-6-backend-development.json`、`Worknotes/stage-7-integration.json`、`docs/tech-stack.md`、`docs/fullstack-architecture.md` 和 `docs/task_format_spec.md`，全面理解上线需求，并生成一个结构化的任务列表。
  2. **填充主JSON文件:** `sprint-prioritizer` 将生成的任务列表（此时 `task_document_path` 字段为空）填充到 `Worknotes/stage-8-integration-deployment-and-launch.json` 文件的 `tasks` 数组中。**生成的每个任务都必须严格按照 `docs/task_format_spec.md` 文件中定义的JSON结构进行创建，包含 `id`, `name`, `description`, `agent`, `status`, `dependencies`, `outputs`, 和 `history` 字段。此清单必须按依赖关系和优先级排序，每个子任务的 `status`字段初始值必须为 `pending`。**
  3. **创建独立任务文档并更新路径:** 对于 `Worknotes/stage-8-integration-deployment-and-launch.json` 中 `tasks` 数组的**每一个任务**，`sprint-prioritizer` 必须执行以下操作：
     * **创建独立Markdown文件:** 在 `Worknotes/tasks/stage8/` 目录下创建一个独立的 Markdown 文件，文件名应清晰地反映任务内容（例如 `setup-ci-cd-pipeline.md`）。
     * **填充文档内容:** 从所有相关源文档中提取并整合与该任务**直接相关**的所有信息，写入新创建的 Markdown 文件中。内容应包括详细的步骤、配置信息、相关脚本和预期的输出。
     * **更新主JSON文件:** `sprint-prioritizer` 必须将新创建的 Markdown 文件的相对路径更新到 `Worknotes/stage-8-integration-deployment-and-launch.json` 文件中对应任务的 `task_document_path` 字段。这个过程是逐一完成的，确保每个任务都有一个链接到其详细上下文的文档。

#### **指令 2: 执行部署与上线**

* **目标:** 将应用部署到生产环境，建立监控，并正式发布。
* **智能体:** `studio-coach`, `api-tester`, `devops-automator`, `infrastructure-maintainer`, `project-shipper`
* **智能体都要输入:** `Worknotes/stage-8-integration-deployment-and-launch.json`
* **任务执行 (协作流程):**
  1. **端到端测试 (E2E Testing):**
     * 指令 **`api-tester`**：“读取 `docs/api-spec.md`、`Worknotes/stage-6-backend-development.json` 和 `Worknotes/stage-7-integration.json`。针对已完成集成的应用，执行一次完整的端到端测试，模拟真实用户的核心操作流（如注册->登录->核心功能->退出）。验证从UI到数据库的完整链路。将详尽的测试报告保存到 `testing/e2e-report.md`。”
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
* **交接文档更新:** `project-shipper` 在应用成功上线后，必须读取 `Worknotes/stage-8-integration-deployment-and-launch.json`，在 `summary` 字段中记录最终的生产应用URL和项目成功上线的最终声明，然后将更新后的 JSON 对象写回文件。