```yaml
number: 2560
title: ty has poor type support for pydantic.
type: issue
state: closed
author: Airpy
labels: []
assignees: []
created_at: 2026-01-19T09:15:40Z
updated_at: 2026-01-19T09:33:42Z
url: https://github.com/astral-sh/ty/issues/2560
synced_at: 2026-01-19T10:28:24Z
```

# ty has poor type support for pydantic.

---

_@Airpy_

### Summary

my pydantic model:
```py
class CreateTestTaskRequest(BaseModel):
    model_config = ConfigDict(extra="forbid")

    project_id: str = Field(..., description="项目ID", alias="projectId")
    app_id: str = Field(..., description="应用ID", alias="appId")
    task_name: str = Field(..., description="测试任务", alias="taskName")
    device_type: int = Field(default=DeviceType.INSTANCE_ID.value, description="设备类型", alias="deviceType")
    device_id: str = Field(..., description="设备ID", alias="deviceId")
```

my test case
```python
create_task_data = CreateTestTaskRequest(
                projectId=onetrack_config.onetrack_config.project_id,
                appId=onetrack_config.onetrack_config.app_id,
                task_name=task_name,
                device_type=device_type_value,
                deviceId=device_info[device_type],
            )
```

command line 
```bash
uvx ty check

error[unknown-argument]: Argument `task_name` does not match any known parameter
   --> src/quality_genie/plugins/event_tracking/test.py:185:17
    |
183 |                 projectId=onetrack_config.onetrack_config.project_id,
184 |                 appId=onetrack_config.onetrack_config.app_id,
185 |                 task_name=task_name,
    |                 ^^^^^^^^^^^^^^^^^^^
186 |                 device_type=device_type_value,
187 |                 deviceId=device_info[device_type],
    |
info: rule `unknown-argument` is enabled by default
```

### Version

"ty==0.0.12",

---

_Comment by @MichaReiser on 2026-01-19 09:33_

We plan to add dedicated pydantic support in the near future. See https://github.com/astral-sh/ty/issues/2403

---

_Closed by @MichaReiser on 2026-01-19 09:33_

---
