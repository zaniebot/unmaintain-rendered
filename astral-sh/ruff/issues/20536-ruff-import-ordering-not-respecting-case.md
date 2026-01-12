```yaml
number: 20536
title: "Ruff Import ordering not respecting case-insensitivity compared to flake8 `import-order-style`"
type: issue
state: closed
author: SkylerWittman
labels:
  - documentation
assignees: []
created_at: 2025-09-23T14:57:52Z
updated_at: 2025-09-25T16:25:13Z
url: https://github.com/astral-sh/ruff/issues/20536
synced_at: 2026-01-12T15:54:57Z
```

# Ruff Import ordering not respecting case-insensitivity compared to flake8 `import-order-style`

---

_@SkylerWittman_

### Summary

Hi there,

I'm trying to fully replace `isort` and `flake8` with `ruff`. Previously with flake8 our team had used this import order style `import-order-style = appnexus` and I'm trying to simulate the same in `ruff`. 

I cannot seem to figure out how to make the imports from the same file be case-insensitive and alphabetically sorted. The `tool.ruff.lint.isort` setting `case-sensitive` is default `false` which you would think would leave this import unchanged but it does not.

This is what the import used to look like before running `ruff check --fix`
`from pydantic import BaseModel, computed_field, Field`

After running `ruff`:

`from pydantic import BaseModel, Field, computed_field`

Here is my full config for reference and I am running it as `ruff check --fix /path/to/file.py`

```toml
[tool.ruff]
exclude = ["__pycache__", ".venv", ".vscode", "**/tests/**"]

line-length = 160
indent-width = 4

[tool.ruff.lint]
select = [
    "E", # pycodestyle errors
    "W", # pycodestyle warnings
    "F", # pyflakes
    "N", # pep8-naming
]

# run isort import sorting on top of the default linting rules 
extend-select = ["I"]

[tool.ruff.lint.per-file-ignores]
"src/netsuite/soap_helper.py" = ["E221", "E231", "E222"]
"src/netsuite/soap_mixin.py" = ["E231"]
"src/netsuite/records/customer.py" = ["E231"]
"src/job.py" = ["E231"]
"src/account.py" = ["E231"]

[tool.ruff.lint.isort]
force-sort-within-sections = true
force-wrap-aliases = true
combine-as-imports = true
lines-after-imports = 2

section-order = [
    "future",
    "standard-library",
    "third-party",
    "first-party",
    "local-folder",
]

known-first-party = ["stingray"]
known-third-party = [
    "aws",
    "aws_lambda_powertools",
    "aws_xray_sdk",
    "boto3",
    "pydantic",
    "requests",
    "yattag",
]
known-local-folder = ["config", "netsuite"]

[tool.ruff.format]
line-ending = "auto"
indent-style = "space"
quote-style = "single"
```

### Version

_No response_

---

_Comment by @ntBre on 2025-09-23 19:16_

It looks like the [order-by-type](https://docs.astral.sh/ruff/settings/#lint_isort_order-by-type) setting takes precedence over `case-sensitive`. If you disable that too, you can achieve the ordering you had originally in this example: [playground](https://play.ruff.rs/e106a249-63de-476a-99b9-a2998328da5f).

---

_Label `question` added by @ntBre on 2025-09-23 19:16_

---

_Comment by @SkylerWittman on 2025-09-23 19:36_

Thank you so much @ntBre that is exactly what I needed.

I've updated my toml to 
```toml
order-by-type = false
case-sensitive = false
```

Is it worth it to update the docs to hyperlink [order-by-type](https://docs.astral.sh/ruff/settings/#lint_isort_order-by-type) back to [case-sensitive](https://docs.astral.sh/ruff/settings/#lint_isort_case-sensitive) so that it's easier to find? I would be happy to make that PR.

---

_Comment by @ntBre on 2025-09-23 20:01_

Sure! This interaction was surprising to me, so it seems like a good thing to document :) It might make sense to link in both directions (`order-by-type` -> `case-sensitive` and `case-sensitive` -> `order-by-type`) too.

---

_Label `documentation` added by @ntBre on 2025-09-23 20:01_

---

_Label `question` removed by @ntBre on 2025-09-23 20:01_

---

_Comment by @SkylerWittman on 2025-09-24 01:24_

@ntBre I've cloned the repo locally and created a branch but I am not allowed to push the branch as I'm missing permission, is there a formal request process to request the ability create a PR?
I didn't see anything specific mentioned in the Contributing section of the README.

If it's easier for you or someone else to create a PR and generate the ruff.schema.json (I am not setup for Rust dev on my machine) then I will attach my git patch.

Thanks!

[0001-Clarify-dependency-between-order_by_type-and-case_se.patch](https://github.com/user-attachments/files/22504420/0001-Clarify-dependency-between-order_by_type-and-case_se.patch)

---

_Comment by @ntBre on 2025-09-24 01:50_

I think you need to fork the repo on GitHub and create a PR from your copy. I'm happy to open a PR from the patch if you'd rather not do that, though. And I can definitely update the schema file in either case!

---

_Comment by @SkylerWittman on 2025-09-24 02:00_

If you don't mind that would be swell @ntBre.
Cheers

---

_Closed by @ntBre on 2025-09-25 16:25_

---
