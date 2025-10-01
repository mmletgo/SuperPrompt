# 任务JSON结构定义

所有在 `Worknotes/` 目录中生成的任务都必须遵循以下JSON结构。

```json
{
  "tasks": [
    {
      "id": "string",
      "name": "string",
      "description": "string",
      "agent": "string",
      "status": "string",
      "dependencies": ["string"],
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