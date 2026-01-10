---
number: 9999
title: "Feature Request: Ability to Assign Index to Dependency Groups with Priority Management"
type: issue
state: open
author: andy-zmn
labels:
  - configuration
assignees: []
created_at: 2024-12-18T14:23:12Z
updated_at: 2025-01-06T20:45:17Z
url: https://github.com/astral-sh/uv/issues/9999
synced_at: 2026-01-10T01:24:49Z
---

# Feature Request: Ability to Assign Index to Dependency Groups with Priority Management

---

_Issue opened by @andy-zmn on 2024-12-18 14:23_

Description:
I propose a new feature for uv that allows assigning an index to entire dependency groups in the pyproject.toml file. Additionally, I suggest introducing a priority system to resolve conflicts between individual package index assignments and group-based index assignments.

Currently, there is no way to specify an index for a group of dependencies. Instead, each package's index must be defined individually, which can be cumbersome for projects with large configurations. This proposal addresses that limitation while ensuring predictable behavior through explicit or implicit priority rules.

preliminary parameters:
```toml
[dependency-groups]
main-libs = [
    "mysqlclient==2.*",
    "requests==2.*",
]

helper-libs = [
    "first-popular-lib-name==1.0.0",
    "other-popular-lib-name==0.3.0",
]

[tool.uv]
default-groups = ["main-libs", "helper-libs"]

[[tool.uv.index]]
name = "local-development"
url = "https://nexus.local/repository/pip-proxy/simple"
default = true

[[tool.uv.index]]
name = "helpers"
url = "https://nexus.local/repository/pip-helpers/simple"
explicit = true
```
Current Alternative:
Currently, the index must be specified for each package individually:

```toml
[tool.uv.sources]  
first-popular-lib-name = { index = "helpers" }  
other-popular-lib-name = { index = "helpers" }
```

Assigning an index to a dependency group
proposed syntax
```toml
[tool.uv.sources.groups]  
helper-libs = { index = "helpers", priority = 2 } 
```

Assigning an index to an individual package with priority
proposed syntax
```toml
[tool.uv.sources]  
first-popular-lib-name = { index = "local-development", priority = 1 }  
```

Behavior and Priority Rules

- Default Priority (Implicit): If priority is not specified, the configuration is parsed top to bottom. Earlier definitions take precedence over later ones.
- Explicit Priority: A lower priority value indicates a higher precedence.
- Tie-Breaking: If two sources or groups have the same priority, the order of their definitions in the file determines precedence.

Examples
1. Explicit Priority Resolution
```toml
[dependency-groups]  
helper-libs = [  
    "first-popular-lib-name==1.0.0",  
    "other-popular-lib-name==0.3.0",
]  

[tool.uv.sources]  
first-popular-lib-name = { index = "local-development", priority = 1 }  

[tool.uv.sources.groups]  
helper-libs = { index = "helpers", priority = 2 }  
```
`first-popular-lib-name` will use the `local-development` index, as it has a higher priority (`priority = 1`) than the group it belongs to (`priority = 2`).
Other packages in `helper-libs` will use the `helpers` index.

2. Default Parsing Order
```toml
[tool.uv.sources.groups]  
helper-libs = { index = "helpers" }  

[tool.uv.sources]  
first-popular-lib-name = { index = "local-development" }  
```
In this case, `first-popular-lib-name` will use the `helpers` index because the group assignment appears earlier in the file.

Benefits:
- Simplified Configuration: Group-level index assignments reduce redundancy in large projects.
- Predictable Behavior: Explicit and implicit priority rules provide clarity in conflict resolution.
- Backward Compatibility: The feature does not alter the behavior of existing configurations.

Edge Cases and Solutions
1. Overriding Behavior:
The feature relies on clear rules for resolving conflicts. Both implicit (order-based) and explicit (priority-based) mechanisms ensure flexibility and control.

2. Documentation and Tooling
Detailed documentation and clear error/warning messages during parsing would help users understand and debug complex configurations.

Thank you for considering this feature!


---

_Label `configuration` added by @zanieb on 2025-01-06 20:45_

---
