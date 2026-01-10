```yaml
number: 2337
title: Per module override ?
type: issue
state: closed
author: Pijukatel
labels:
  - question
assignees: []
created_at: 2026-01-05T09:37:31Z
updated_at: 2026-01-05T09:49:32Z
url: https://github.com/astral-sh/ty/issues/2337
synced_at: 2026-01-10T01:56:41Z
```

# Per module override ?

---

_Issue opened by @Pijukatel on 2026-01-05 09:37_

### Question

Hello, we would like to migrate from **mypy** to **ty**, but I failed to find how to configure the **ty**  with per-module specific overrides

In **mypy** I would use per-module overrides:
https://mypy.readthedocs.io/en/stable/config_file.html#per-module-and-global-options

In **ty** I found only per-file overrides:
https://docs.astral.sh/ty/reference/configuration/#include
 

Actual usecase is handling of **unresolved-import** only for specific modules, not for the whole file.

Example:

```
import missing_module # I want to ignore this error, the module is not installed and I know it.
import existing_module_with_typo # I want to catch this error, this is typo

...

```
(We can have many files in our code base like this, so putting specific ignore on each line is not very good, especially since it is example code for documentation.)



### Version

_No response_

---

_Label `question` added by @Pijukatel on 2026-01-05 09:37_

---

_Comment by @AlexWaygood on 2026-01-05 09:49_

Hi! Please see https://github.com/astral-sh/ty/issues/1354 and https://github.com/astral-sh/ty/issues/2082

---

_Closed by @AlexWaygood on 2026-01-05 09:49_

---

_Comment by @MichaReiser on 2026-01-05 09:49_

No, this isn't currently supported but is something we plan on adding https://github.com/astral-sh/ty/issues/1354

---
