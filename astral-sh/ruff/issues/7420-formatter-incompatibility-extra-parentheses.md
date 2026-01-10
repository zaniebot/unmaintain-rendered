```yaml
number: 7420
title: "Formatter incompatibility: extra parentheses around `await` assignment"
type: issue
state: closed
author: charliermarsh
labels:
  - formatter
  - accepted
assignees: []
created_at: 2023-09-15T20:53:05Z
updated_at: 2023-09-16T17:10:36Z
url: https://github.com/astral-sh/ruff/issues/7420
synced_at: 2026-01-10T11:09:49Z
```

# Formatter incompatibility: extra parentheses around `await` assignment

---

_Issue opened by @charliermarsh on 2023-09-15 20:53_

Given:

```python
result = await self.request(
    f"/applications/{int(application_id)}/guilds/{int(scope)}/commands/{int(command_id)}/permissions"
)
```

Ruff formats as:

```python
result = (
    await self.request(
        f"/applications/{int(application_id)}/guilds/{int(scope)}/commands/{int(command_id)}/permissions"
    )
)
```

While Black formats as:

```python
result = await self.request(
    f"/applications/{int(application_id)}/guilds/{int(scope)}/commands/{int(command_id)}/permissions"
)
```

Sourced from: https://github.com/astral-sh/ruff/issues/7394.


---

_Label `formatter` added by @charliermarsh on 2023-09-15 20:53_

---

_Comment by @MichaReiser on 2023-09-15 21:01_

This could be because the awaited expression exceeds the line width. We would need to use best fitting if we want to avoid this for unbreakable expressions. But that's just an initial guess. It could also be that we incorrectly do not apply the can omit optional parentheses layout 

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-15 21:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-15 21:21_

---

_Label `accepted` added by @charliermarsh on 2023-09-15 21:21_

---

_Comment by @MichaReiser on 2023-09-16 14:37_

Closing as duplicate of #7431 (They have the same root cause)

---

_Closed by @MichaReiser on 2023-09-16 14:37_

---

_Reopened by @MichaReiser on 2023-09-16 14:39_

---

_Closed by @charliermarsh on 2023-09-16 17:10_

---
