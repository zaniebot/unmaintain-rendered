```yaml
number: 18647
title: "[`isort-I001`] Discrepancy between local ruff and GitHub action"
type: issue
state: closed
author: gstrauss-zscaler
labels:
  - needs-info
assignees: []
created_at: 2025-06-12T13:56:48Z
updated_at: 2025-06-18T20:16:13Z
url: https://github.com/astral-sh/ruff/issues/18647
synced_at: 2026-01-12T15:54:56Z
```

# [`isort-I001`] Discrepancy between local ruff and GitHub action

---

_@gstrauss-zscaler_

### Summary

Hey,
We currently have ruff as pre-commit hook for our project.
We inserted a GitHub Action to automate lint checking upon pull request

Although running `ruff check` and `ruff check --select I001` return `All checks passed!`
The GitHub Step fails on some files, with `I001 Import block is un-sorted or un-formatted`
I even tried to run `isort <FILE_NAME>` and it passes as well
We are latest version (`ruff 0.11.13 (5faf72a4d 2025-06-05)`)
I don't know how to solve this, is it some issue with the fact that we are running locally with macOS but GitHub Actions runs with linux?

```toml
line-length = 120
target-version = "py312"

[lint]
select = [
    "ALL", # include all the rules, including new ones
]
ignore = [
    "FIX002",
    "TD003",
    "TD002",
    "D104",
    "D101",
    "D102",
    "D205",
    "D401",
    "D100",
    "D103",
    "RUF006",
    "A002",
    "D203",
    "D213",
    "COM812",
    "D400",
    "D415",
    "D107",
]
[lint.per-file-ignores]
    "tests/*" = ["S101", "FBT001"]
```

This is the step
```yaml
  - uses: astral-sh/ruff-action@v3
```
### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Comment by @ntBre on 2025-06-12 14:11_

This sounds related to https://github.com/astral-sh/ruff-pre-commit/issues/121. Do you happen to have a local directory with the same name as one of your dependencies that is not present in CI? I think that can cause the isort rules to miscategorize the imports and cause formatting differences.

If so this could be a duplicate of https://github.com/astral-sh/ruff/issues/10519

---

_Renamed from "Discrepancy between local ruff and GitHub action" to "[isort-I001] Discrepancy between local ruff and GitHub action" by @gstrauss-zscaler on 2025-06-12 14:12_

---

_Renamed from "[isort-I001] Discrepancy between local ruff and GitHub action" to "[`isort-I001`] Discrepancy between local ruff and GitHub action" by @gstrauss-zscaler on 2025-06-12 14:12_

---

_Comment by @gstrauss-zscaler on 2025-06-12 14:18_

> This sounds related to [astral-sh/ruff-pre-commit#121](https://github.com/astral-sh/ruff-pre-commit/issues/121). Do you happen to have a local directory with the same name as one of your dependencies that is not present in CI? I think that can cause the isort rules to miscategorize the imports and cause formatting differences.
> 
> If so this could be a duplicate of [#10519](https://github.com/astral-sh/ruff/issues/10519)

Thanks for the quick reply,
I am not sure, from a quick look I don't see anything like this. But how can I check?
So, if I am running `uv sync` and then manually running `ruff check` in the ci/cd. It should fix the error?

---

_Comment by @MichaReiser on 2025-06-12 14:32_

Could you share a minified file that gets changed in CI. E.g how does the ordering differ? 

---

_Label `needs-info` added by @MichaReiser on 2025-06-12 14:33_

---

_Comment by @dylwil3 on 2025-06-12 15:57_

If it's not too difficult to do, you might try running both locally and in CI in verbose mode once (maybe even hack it to only run on a specific file where the issue occurs), like this:

```console
ruff check --select I001 -v path/to/problematic/file.py
```

This should print out Ruff's decision about how to categorize each of the imports and the reason behind the categorization.

`isort` also offers a verbose mode, so you could print out both to see where Ruff is making a different categorization.

---

_Comment by @gstrauss-zscaler on 2025-06-15 07:50_

Thanks everyone, found the issue
We are using protobuf and we only generate the python files in the dockerfile.
So ruff is thinking it's a third package, while it's not

Added this to my ruff.toml
```toml
[lint.isort]
# Our own generated protobuf
known-first-party = ["protobuf"]
```

---

_Closed by @gstrauss-zscaler on 2025-06-15 07:50_

---

_Comment by @brownterryn on 2025-06-18 20:16_

Or if you're putting it in `pyproject.toml`:
```toml
[tool.ruff.lint.isort]
# Our own generated protobuf
known-first-party = ["protobuf"]
```

---
