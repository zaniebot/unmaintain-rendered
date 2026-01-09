---
number: 7839
title: Support for validate-pyproject
type: issue
state: open
author: henryiii
labels:
  - configuration
assignees: []
created_at: 2023-10-06T17:13:18Z
updated_at: 2023-10-28T09:00:00Z
url: https://github.com/astral-sh/ruff/issues/7839
synced_at: 2026-01-07T13:12:15-06:00
---

# Support for validate-pyproject

---

_Issue opened by @henryiii on 2023-10-06 17:13_

Would it be possible to support [validate-pyproject](https://validate-pyproject.readthedocs.io/en/latest/)? There already is a schema at https://github.com/astral-sh/ruff/blob/main/ruff.schema.json. All that would be needed would be:


```toml
[project.entry-points]
"validate_pyproject.tool_schema".ruff = "ruff.schema:get_schema"
```

And a `ruff.schema.get_schema` (name arbitrary) function something like this:

```python
def get_schema(tool_name: str = "ruff") -> dict[str, Any]:
    "Get the stored complete schema for ruff settings."
    assert tool_name == "ruff", "Only ruff is supported."

    with resources.joinpath("ruff.schema.json").open(encoding="utf-8") as f:
        return json.load(f)  # type: ignore[no-any-return]
```

(Assuming there's a way to ship the schema in the python package, currently it's at the root of the repo) That would allow validate-pyproject to validate Ruff's config if Ruff was installed in the same environment as validate-pyproject. Thoughts?

(I'm also hoping that validate-pyproject can use SchemaStore eventually, but that's notably usually out of date for Ruff)

---

_Comment by @charliermarsh on 2023-10-06 17:58_

I assume so, this seems like a nice thing to do.

---

_Label `configuration` added by @charliermarsh on 2023-10-08 14:41_

---

_Comment by @baggiponte on 2023-10-28 09:00_

+1 on this one! I reckon this might be a topic for a separate issue, but tox devs are also working on [pyproject-fmt](https://pyproject-fmt.readthedocs.io/en/latest/)

---

_Referenced in [astral-sh/uv#15006](../../astral-sh/uv/issues/15006.md) on 2025-07-31 19:35_

---
