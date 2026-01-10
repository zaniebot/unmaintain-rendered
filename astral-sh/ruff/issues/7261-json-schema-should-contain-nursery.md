```yaml
number: 7261
title: "JSON schema should contain `\"NURSERY\"`"
type: issue
state: closed
author: bersbersbers
labels:
  - bug
assignees: []
created_at: 2023-09-10T16:22:07Z
updated_at: 2023-09-14T18:09:37Z
url: https://github.com/astral-sh/ruff/issues/7261
synced_at: 2026-01-10T11:09:49Z
```

# JSON schema should contain `"NURSERY"`

---

_Issue opened by @bersbersbers on 2023-09-10 16:22_

`"NURSERY"` is missing here
https://github.com/astral-sh/ruff/blob/9cb5ce750e9dead53eb8235e8cfb7cff653a1cd2/ruff.schema.json#L1683-L2754

leading to errors such as this:

![image](https://github.com/astral-sh/ruff/assets/12128514/1138220b-6dac-4418-b89e-60e2bc5ba515)


---

_Comment by @zanieb on 2023-09-10 17:02_

This may be addressed by #7195 

---

_Comment by @bersbersbers on 2023-09-11 18:39_

> This may be addressed by #7195

Doesn't look like it to me: https://github.com/astral-sh/ruff/commit/6566d00295b93af489e0d64d26d05138f5500f00#diff-0a03bc8f85db936902b6bfe61e0893160ee1c00de4253de91aa4fdb00202d73f doesn't seem to add `"NURSERY"` nor `"PREVIEW"`.

---

_Comment by @charliermarsh on 2023-09-11 18:41_

I think these are manually excluded. The original intent was to hide them from autocompleting since we wanted to discourage ALL usage. But it ends up being too confusing that they’re marked as errors (we should fix).

---

_Comment by @zanieb on 2023-09-11 18:44_

I think we'll want to exclude NURSERY since it's deprecated but include PREVIEW — I need to do a bit more work to decide if a PREVIEW selector makes sense though.

---

_Comment by @charliermarsh on 2023-09-11 18:45_

In the case of nursery… I’m not sure — it seems wrong to mark a valid value as invalid.

---

_Comment by @zanieb on 2023-09-11 18:49_

It's only valid as in "if you are using it currently we will not break your workflow on the next release" — I hope to remove it entirely soon. I could go either way on including it in the schema but would generally prefer to push people not to use it. Perhaps we can use a `deprecated` keyword per https://github.com/json-schema-org/json-schema-spec/pull/737

If someone wants to look into support for marking schema items as deprecated I'd be curious to review :)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-14 00:43_

---

_Label `bug` added by @charliermarsh on 2023-09-14 00:44_

---

_Closed by @charliermarsh on 2023-09-14 18:09_

---
