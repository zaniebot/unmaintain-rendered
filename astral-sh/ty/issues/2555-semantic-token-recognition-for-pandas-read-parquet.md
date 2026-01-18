```yaml
number: 2555
title: Semantic token recognition for pandas.read_parquet
type: issue
state: open
author: mlisovyi
labels:
  - server
assignees: []
created_at: 2026-01-18T14:33:47Z
updated_at: 2026-01-18T14:34:52Z
url: https://github.com/astral-sh/ty/issues/2555
synced_at: 2026-01-18T15:23:08Z
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
