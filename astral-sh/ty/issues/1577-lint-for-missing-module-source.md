```yaml
number: 1577
title: lint for missing-module-source
type: issue
state: open
author: MichaReiser
labels:
  - lint
assignees: []
created_at: 2025-11-17T13:21:21Z
updated_at: 2025-11-17T17:07:03Z
url: https://github.com/astral-sh/ty/issues/1577
synced_at: 2026-01-10T02:06:25Z
```

# lint for missing-module-source

---

_Issue opened by @MichaReiser on 2025-11-17 13:21_

At least Pyrefly and Pyright both support linting for modules where the typing stubs can be found, but the runtime modules are missing:

> reportMissingModuleSource [boolean or string, optional]: Generate or suppress diagnostics for imports that have no corresponding source file. This happens when a type stub is found, but the module source file was not found, indicating that the code may fail at runtime when using this execution environment. Type checking will be done using the type stub. The default value for this setting is "warning". 
> https://microsoft.github.io/pyright/#/configuration?id=type-check-diagnostics-settings

---

_Label `lint` added by @MichaReiser on 2025-11-17 13:21_

---

_Comment by @AlexWaygood on 2025-11-17 17:07_

I agree that this is a useful lint that we should implement.

Ideally we would suppress the lint in stub files `if TYPE_CHECKING` blocks (the latter would require https://github.com/astral-sh/ty/issues/1553).

We should also consider whether we want it enabled by default or not. It's quite common (especially in CI) to type-check a project with only stubs installed, without runtime sources available -- the reason is that stubs are almost always pure-Python packages, which can be much faster and easier to install than packages with large C extensions. This lint might make sense as a disabled-by-default error code for users to explicitly opt into.

---
