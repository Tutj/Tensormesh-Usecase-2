# Data Model: LMCache Huge Data Access Tutorial

## Entities

### TutorialState (LangGraph State)
Represents the state passed between nodes in the LangGraph.
- `manual_text`: String. The large static content (the Manual).
- `agent_instruction`: String. The dynamic instruction for the LLM.
- `user_query`: String. The specific question from the user.
- `full_prompt`: String. The final concatenated prompt sent to the LLM.
- `metrics`: Dictionary. Contains `ttft`, `hit_rate`, and `total_time`.

### InferenceResult
The response received from the vLLM + LMCache engine.
- `text`: String. The generated response.
- `usage`: Dictionary. Token counts.
- `performance`: Dictionary. `time_to_first_token`, `cache_hit_status`.

### MetricPoint
A single data point for visualization.
- `scenario_name`: String (e.g., "Cold Start", "Optimal Cache", "Cache Busted").
- `ttft`: Float.
- `hit_rate`: Float.

## Relationships
- `TutorialState` is updated by nodes: `SetupNode` -> `InstructionNode` -> `InferenceNode`.
- `InferenceResult` is used to populate the `metrics` field in `TutorialState`.
- `MetricPoint` is derived from multiple `TutorialState` instances for final plotting.
