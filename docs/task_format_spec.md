# 任务JSON结构定义

所有在 `Worknotes/` 目录中生成的任务都必须遵循以下JSON结构。

```json
{
  "tasks": [
    {
      "id": "string",
      "name": "string",
      "description": "string",
      "task_document_path": "string",
      "agent": "string",
      "status": "string",
      "dependencies": ["string"],
      "outputs": [
        {
          "type": "string",
          "path": "string",
          "description": "string"
        }
      ],
      "history": [
        {
          "timestamp": "string",
          "status": "string",
          "notes": "string"
        }
      ]
    }
  ],
  "summary": "string",
  "next_stage_suggestions": "string"
}
```

## 字段说明

### `tasks` (Array)
任务列表，包含一个或多个任务对象。

### 任务对象字段

*   **`id` (string):**
    *   **作用:** 任务的唯一标识符。
    *   **格式:** 建议使用 `stageX-taskY` 的格式，例如 `stage2-task1`。

*   **`name` (string):**
    *   **作用:** 任务的简短名称，清晰概括任务内容。
    *   **示例:** "创建用户认证API"。

*   **`description` (string):**
    *   **作用:** 对任务的详细描述，说明任务的目标、范围和关键要求。

*   **`task_document_path` (string):**
    *   **作用:** 指向一个Markdown文件，该文件包含了执行此任务所需的所有上下文信息、需求和规范。
    *   **目的:** 避免子智能体在执行具体任务时需要读取大量通用文档，为每个任务提供一个精确、独立的上下文环境。
    *   **格式:** 必须是一个指向 `Worknotes/` 目录下某个文件的相对路径。
    *   **示例:** `Worknotes/tasks/stage5/task1-implement-login-ui.md`。

*   **`agent` (string):**
    *   **作用:** 指定负责执行此任务的智能体名称。
    *   **示例:** `backend-developer`。

*   **`status` (string):**
    *   **作用:** 任务的当前状态。
    *   **可选值:**
        *   `pending`: 待处理
        *   `in_progress`: 进行中
        *   `completed`: 已完成
        *   `blocked`: 已阻塞
        *   `cancelled`: 已取消

*   **`dependencies` (Array of strings):**
    *   **作用:** 此任务依赖的其他任务的 `id` 列表。
    *   **示例:** `["stage2-task1"]`。

*   **`outputs` (Array of objects):**
    *   **作用:** 任务完成后生成的产出物列表。
    *   **每个产出物对象包含:**
        *   `type` (string): 产出物类型 (例如: `file`, `api_endpoint`, `database_schema`)。
        *   `path` (string): 产出物的路径或标识符 (例如: 文件路径, URL)。
        *   `description` (string): 对产出物的简要描述。

*   **`history` (Array of objects):**
    *   **作用:** 记录任务状态变更的历史。
    *   **每个历史记录对象包含:**
        *   `timestamp` (string): 状态变更的时间戳 (ISO 8601 格式)。
        *   `status` (string): 变更后的状态。
        *   `notes` (string, optional): 关于此次状态变更的备注。