---
number: 4107
title: "Docs request: `autoflake` in Rules page"
type: issue
state: closed
author: jamesbraza
labels:
  - question
assignees: []
created_at: 2023-04-26T00:09:05Z
updated_at: 2023-04-26T17:28:47Z
url: https://github.com/astral-sh/ruff/issues/4107
synced_at: 2026-01-07T13:12:14-06:00
---

# Docs request: `autoflake` in Rules page

---

_Issue opened by @jamesbraza on 2023-04-26 00:09_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

https://beta.ruff.rs/docs/rules/ mentions that `ruff` reimplements `autoflake`.

However, in the Rules sidebar (where relevant codes are listed), `autoflake` is not mentioned.

Can we document what codes pertain to `autoflake`?


---

_Comment by @charliermarsh on 2023-04-26 15:12_

Hmm, it doesn't quite fit into our documentation paradigm, because `autoflake` is just a tool that adds autofix behavior for a small subset of the Pyflakes violations -- it doesn't define any violations of its own. I'm not sure that it's important that users know which rules that includes. What's the use motivation? Is this answer here sufficient? :)

---

_Label `question` added by @charliermarsh on 2023-04-26 15:12_

---

_Comment by @charliermarsh on 2023-04-26 17:10_

Closing but let me know if you have any follow-up questions.

---

_Closed by @charliermarsh on 2023-04-26 17:10_

---

_Comment by @jamesbraza on 2023-04-26 17:14_

I am trying to migrate much of my full `pre-commit` config to `ruff`, one of them is `autoflake`.  I see it's mentioned in the `README` as being replace-able, but I am not sure:
- What codes are relevant
- Any configuration necessary

I run `autoflake` like so:

```yaml
  - repo: https://github.com/PyCQA/autoflake
    rev: v2.1.1
    hooks:
      - id: autoflake
        args:
          - --remove-all-unused-imports
          - --remove-rhs-for-unused-variables
          - --remove-unused-variables
          - --exclude=somelocaldir
```

I made this docs request mainly because an `autoflake-to-ruff` (inspired by [`flake8-to-ruff`](https://pypi.org/project/flake8-to-ruff/)) conversion is not clear to me

---

_Comment by @charliermarsh on 2023-04-26 17:17_

No configuration is necessary, it's effectively just "autofix enabled for the F rule set", which is enabled by default (namely `F401`, unused imports, and `F841`, unused local variable).

---

_Comment by @jamesbraza on 2023-04-26 17:27_

Okay gotchu.  I see `F401` and `F841` codes are mentioned under Pyflakes (F): https://beta.ruff.rs/docs/rules/#pyflakes-f

So I guess `autoflake` and `pyflakes` were redundant/have some overlap.  I would say from the current docs, one cannot discern that the autofixes for `F401` and `F841` are equivalent to `autoflake`.

---

Just to explain my process:
1. For each tool I have in `pre-commit`
2. I search `ruff` docs if it's there and can be removed

I saw `autoflake` in the `README`, but I didn't really see anywhere in `ruff` docs how it's replaced.  I still think it's useful to put in the docs what rules (or codes or autofixes) pertain to the tool.

---

_Comment by @jamesbraza on 2023-04-26 17:28_

And I really appreciate `ruff` and what you're doing here!  At PyCon 2023 (last weekend), I heard it mentioned quite a lot, hence why I am making the jump now 👌 

---

_Referenced in [NVIDIA/TensorRT-LLM#8398](../../NVIDIA/TensorRT-LLM/pulls/8398.md) on 2025-10-16 14:48_

---
