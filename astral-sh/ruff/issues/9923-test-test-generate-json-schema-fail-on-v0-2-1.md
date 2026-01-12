```yaml
number: 9923
title: Test test_generate_json_schema fail on v0.2.1
type: issue
state: closed
author: GaetanLepage
labels:
  - bug
assignees: []
created_at: 2024-02-10T10:25:36Z
updated_at: 2024-02-10T11:20:39Z
url: https://github.com/astral-sh/ruff/issues/9923
synced_at: 2026-01-12T15:54:49Z
```

# Test test_generate_json_schema fail on v0.2.1

---

_@GaetanLepage_

The following test fail on the latest release:
```
failures:

---- generate_json_schema::tests::test_generate_json_schema stdout ----
Error: ruff.schema.json changed, please run `cargo dev generate-all`:
```

This is the part of the diff which differs:
```
         "RUF2",
         "RUF20",
         "RUF200",
>        "RUF9",
>        "RUF90",
>        "RUF900",
>        "RUF901",
>        "RUF902",
>        "RUF903",
>        "RUF91",
>        "RUF911",
>        "RUF912",
>        "RUF92",
>        "RUF920",
>        "RUF921",
>        "RUF95",
>        "RUF950",
         "S",
         "S1",
         "S10",
         "S101",
         "S102",
```


---

_Comment by @GaetanLepage on 2024-02-10 10:27_

Seems to work fine on `main` though.

---

_Comment by @charliermarsh on 2024-02-10 11:20_

Yup, I believe this was fixed in #9901. Thanks!

---

_Closed by @charliermarsh on 2024-02-10 11:20_

---

_Label `bug` added by @charliermarsh on 2024-02-10 11:20_

---
