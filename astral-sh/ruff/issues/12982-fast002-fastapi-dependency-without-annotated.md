```yaml
number: 12982
title: "[FAST002] FastAPI dependency without `Annotated` unsafe fix error"
type: issue
state: closed
author: mbrulatout
labels:
  - bug
  - good first issue
  - fixes
  - preview
assignees: []
created_at: 2024-08-19T07:46:28Z
updated_at: 2024-08-28T15:29:01Z
url: https://github.com/astral-sh/ruff/issues/12982
synced_at: 2026-01-12T15:54:52Z
```

# [FAST002] FastAPI dependency without `Annotated` unsafe fix error

---

_@mbrulatout_

Hello,

Using ruff 0.6.1, I keep getting an error on FAST002 autofix.

I tried to simplify as much as i could both the code and config

```python
datacenter_state_router = APIRouter()

@datacenter_state_router.patch(
    "/{name}",
)
async def datacenter_state_patch(
    current_state: DatacenterState = PermissionDepends(Permission.WRITE, get_datacenter_state),
    session: Session = Depends(database_manager.get_session),
) -> DatacenterState:
    pass

```

pyproject.toml


```toml
[tool.ruff]
target-version = 'py311'
line-length = 120
exclude = [
    "criteo/ingress/api/lib/models/alembic/"
]

[tool.ruff.format]
preview = true

[tool.ruff.lint]
# See complete list : https://beta.ruff.rs/docs/rules
select = [
    "FAST",  # fastapi
]

fixable = [
    "FAST002",
]
```

Removing that PermissionDepends arg (a wrapper around an actual dependency) fixes it.

```python

class Permission(Enum):
    READ = "read"
    WRITE = "write"

def permission_dependency(
    permission: Permission, resource: Callable[..., Any], user: User = Depends(get_current_user)
) -> Any:
    dependable_resource = Depends(resource)

    def resource_call(_resource: OwnedResource = dependable_resource, _user: User = user) -> Any:
        return _resource

    return Depends(resource_call)

# An alias to mimic FastAPI Dependency (https://fastapi.tiangolo.com/reference/dependencies/#fastapi.Depends)
# This is usable with the additional permission, the contract being the following:
# PermissionDepends(Permission, Callable[..., OwnedResource])
PermissionDepends = permission_dependency
```


---

_Renamed from "[Fix error] FAST002 FastAPI dependency without `Annotated` unsafe fix" to "[FAST002] FastAPI dependency without `Annotated` unsafe fix" by @mbrulatout on 2024-08-19 07:51_

---

_Renamed from "[FAST002] FastAPI dependency without `Annotated` unsafe fix" to "[FAST002] FastAPI dependency without `Annotated` unsafe fix error" by @mbrulatout on 2024-08-19 07:51_

---

_Comment by @MichaReiser on 2024-08-19 08:07_

For reproduction, it's important that the code has the relevant imports:

```python
from fastapi import Depends, FastAPI, APIRouter

datacenter_state_router = APIRouter()

@datacenter_state_router.patch(
    "/{name}",
)
async def datacenter_state_patch(
    current_state: DatacenterState = PermissionDepends(
        Permission.WRITE, get_datacenter_state
    ),
    session: Session = Depends(database_manager.get_session),
) -> DatacenterState:
    pass

```

[Playground](https://play.ruff.rs/49700529-ffa1-49aa-8305-3c306531f919)

---

_Comment by @MichaReiser on 2024-08-19 08:09_

The fix transforms the example into

```python
from fastapi import Depends, FastAPI, APIRouter
from typing import Annotated

datacenter_state_router = APIRouter()

@datacenter_state_router.patch()
async def datacenter_state_patch(
    current_state: DatacenterState = PermissionDepends(
        Permission.WRITE, get_datacenter_state
    ),
    session: Annotated[Session, Depends(database_manager.get_session)],
) -> DatacenterState:
    pass
```

and the problem is that `session` now no longer has a default value but comes after a parameter with a default value.

The easiest fix is to disable the fix if the parameter comes after a parameter with a default value.

---

_Label `bug` added by @MichaReiser on 2024-08-19 08:09_

---

_Label `fixes` added by @MichaReiser on 2024-08-19 08:09_

---

_Label `preview` added by @MichaReiser on 2024-08-19 08:09_

---

_Label `good first issue` added by @MichaReiser on 2024-08-19 08:09_

---

_Closed by @AlexWaygood on 2024-08-28 15:29_

---
