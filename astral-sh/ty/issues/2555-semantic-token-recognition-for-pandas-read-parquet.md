```yaml
number: 2555
title: Semantic token recognition for pandas.read_parquet
type: issue
state: closed
author: mlisovyi
labels: []
assignees: []
created_at: 2026-01-18T14:33:47Z
updated_at: 2026-01-18T17:40:03Z
url: https://github.com/astral-sh/ty/issues/2555
synced_at: 2026-01-18T18:15:23Z
```

# Semantic token recognition for pandas.read_parquet

---

_@mlisovyi_

### Summary

Token type of the commonly used pandas.read_xxx functions is not "function":

<img width="344" height="47" alt="Image" src="https://github.com/user-attachments/assets/c2ba5d1a-aca9-41da-b59e-7b625449820a" />

_Note that `read_csv` is highlighted as a function, but `read_parquet` or `read_json` not_

Here is the token type as reconstructed by ty:

<img width="449" height="469" alt="Image" src="https://github.com/user-attachments/assets/9d69c577-81c8-4e6b-bd96-575359db30bb" />

### Version

ty 0.0.12 (4b74e4ded 2026-01-14)

---

_Renamed from "Semantic token reconnition for pandas.read_parquet" to "Semantic token recognition for pandas.read_parquet" by @AlexWaygood on 2026-01-18 14:34_

---

_Label `server` added by @AlexWaygood on 2026-01-18 14:34_

---

_Comment by @AlexWaygood on 2026-01-18 17:39_

Thanks for the report!

I can reproduce this locally if I have only `pandas` installed in a virtual environment, but not if I also have `pandas-stubs` installed in my environment. I'd recommend also installing `pandas-stubs` for local development, especially if you're interested in using ty as a type checker as well as a language server -- it will give ty much better type information to work with.

The reason why we do not display the correct semantic token for `read_parquet` when `pandas-stubs` is not installed is because we infer the type of `read_parquet` as `Unknown`. The reason why we infer the type as `Unknown` is because on the 2.3.3 tag of `pandas` (things look a bit different on the `main` branch), `read_parquet` is [decorated](https://github.com/pandas-dev/pandas/blob/9c8bc3e55188c8aff37207a74f1dd144980b8874/pandas/io/parquet.py#L500-L501) with `@doc(storage_options=_shared_docs["storage_options"])`. If we look at the [definition](https://github.com/pandas-dev/pandas/blob/9c8bc3e55188c8aff37207a74f1dd144980b8874/pandas/util/_decorators.py#L343) of the `doc` decorator, we can see that it has this signature:

```py
def doc(*docstrings: None | str | Callable, **params) -> Callable[[F], F]:
```

which means that the reason we infer `Unknown` for `read_parquet` is #1136

---

_Closed by @AlexWaygood on 2026-01-18 17:39_

---

_Label `server` removed by @AlexWaygood on 2026-01-18 17:40_

---
