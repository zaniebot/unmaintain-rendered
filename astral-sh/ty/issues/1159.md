```yaml
number: 1159
title: "Correctly handle overloads of `pydantic.Field`"
type: issue
state: open
author: AndreuCodina
labels:
  - bug
  - overloads
assignees: []
created_at: 2025-09-09T17:10:29Z
updated_at: 2025-12-03T19:47:17Z
url: https://github.com/astral-sh/ty/issues/1159
synced_at: 2026-01-10T01:56:40Z
```

# Correctly handle overloads of `pydantic.Field`

---

_Issue opened by @AndreuCodina on 2025-09-09 17:10_

### Summary

`ty` can't detect a parameter declared in the docstring. In the next code, `model` isn't detected as a valid parameter:

```python
# python: ">=3.12"
# "ty==0.0.1a20"
# "pydantic==2.11.7"
# "langchain-openai==0.3.32"


from langchain_openai import ChatOpenAI
from pydantic import SecretStr


def main() -> None:
    ChatOpenAI(
        api_key=SecretStr(""),
        model="gpt-5",
    )


if __name__ == "__main__":
    main()
```

### Version

_No response_

---

_Comment by @carljm on 2025-09-09 17:41_

The issue here is not related to docstrings. The problem is that we don't yet support custom dataclass-transform field-specifier class/functions (and specifically their `alias` parameter). The langchain model has a field `model_name: str = Field(..., alias="model")` and a field `openai_api_key: Optional[SecretStr] = Field(..., alias="api_key")`, and we should understand that therefore `model` and `api_key` are valid constructor arguments.

Closing this as a duplicate of https://github.com/astral-sh/ty/issues/1068 

---

_Closed by @carljm on 2025-09-09 17:41_

---

_Reopened by @sharkdp on 2025-12-03 10:27_

---

_Comment by @sharkdp on 2025-12-03 10:41_

I looked into this again today. https://github.com/astral-sh/ruff/pull/20961 added support for `alias`, but we still error on the original example here.

The problem seems to be that the `Field` call in
```py
class BaseChatOpenAI(BaseChatModel):
    # […]
    model_name: str = Field(default="gpt-3.5-turbo", alias="model")
```
is resolved to `Unknown`, whereas it should [pick this overload of `Field`](https://github.com/pydantic/pydantic/blob/bc15b36967e75bfc3aa88b5fb7982076c2133dba/pydantic/fields.py#L1008-L1049). I noticed that everything works as expected if I remove [this fallback overload](https://github.com/pydantic/pydantic/blob/bc15b36967e75bfc3aa88b5fb7982076c2133dba/pydantic/fields.py#L1133-L1171) which doesn't even accept a `default` argument. It does, however, *seem* to accept extra keyword arguments. It has a `**extra: Unpack[_EmptyKwargs]` parameter where `_EmptyKwargs` is an empty `TypedDict`.

So it looks to me like this is blocked on `Unpack` support? (#156 mentions it, but I'm not sure if that is only supposed to track `Unpack` for `TypeVarTuple`?)

---

_Label `bug` added by @sharkdp on 2025-12-03 10:41_

---

_Label `overloads` added by @sharkdp on 2025-12-03 10:41_

---

_Added to milestone `Stable` by @sharkdp on 2025-12-03 10:41_

---

_Renamed from "Detect parameter type in docstring" to "Correctly handle overloads of `pydantic.Field`" by @carljm on 2025-12-03 19:47_

---
