```yaml
number: 14205
title: PT017 triggers on non-test code
type: issue
state: closed
author: ashb
labels: []
assignees: []
created_at: 2024-11-08T14:54:39Z
updated_at: 2024-11-29T13:11:21Z
url: https://github.com/astral-sh/ruff/issues/14205
synced_at: 2026-01-10T11:09:56Z
```

# PT017 triggers on non-test code

---

_Issue opened by @ashb on 2024-11-08 14:54_

I've ran into a case where the "pytest" rules are triggering on my non-tests code. This snippet triggers it (the `noqa` line shows where) 

```python
def raise_on_4xx_5xx(response: httpx.Response):
    return response.raise_for_status()


# Py 3.11+ version
def raise_on_4xx_5xx_with_note(response: httpx.Response):
    try:
        return response.raise_for_status()
    except httpx.HTTPStatusError as e:
        if TYPE_CHECKING:
            assert hasattr(e, "add_note")  # noqa: PT017
        e.add_note(
            f"Correlation-id={response.headers.get('correlation-id', None) or response.request.headers.get('correlation-id', 'no-correlction-id')}"
        )
        raise


if hasattr(BaseException, "add_note"):
    # Py 3.11+
    raise_on_4xx_5xx = raise_on_4xx_5xx_with_note
```

(I know I can disable `PT` for non-test files as a work around. )

---

_Comment by @dylwil3 on 2024-11-08 21:53_

Thanks for bringing this up! I think this is a place where more documentation might be helpful.

To the best of my understanding, there is currently no way for Ruff to know which functions are tests and which are not. This is mainly because users can [customize the naming convention for tests](https://docs.pytest.org/en/stable/example/pythoncollection.html#changing-naming-conventions) and Ruff wouldn't know.

As you point out, you can get around this limitation by putting a `ruff.toml` file in your `tests` directory (assuming that is your layout) that has:

```toml
[lint]
extend-select = ["PT"]
```

Added: You can verify that the same behavior happens with `flake8` itself

```python
# tmp.py
def foo():
    try:
        ...
    except Exception as e:
        assert e
```

```console
$ uv tool run --with flake8-pytest-style flake8 --select PT tmp.py
tmp.py:5:9: PT017 found assertion on exception e in except block, use pytest.raises() instead
```

---

_Closed by @dylwil3 on 2024-11-11 04:48_

---

_Comment by @LordAro on 2024-11-29 13:11_

This affects PT018 as well, for reference.

Is there any particular reason why ruff couldn't also provide a similar configuration option? It could even look for pytest options in pyproject.toml...

We have a large monorepo with lots of individual test directories in it but a single pyproject.toml controlling everything, so adding a ruff.toml file per test directory would get infeasible quickly

---
