```yaml
number: 13051
title: "`UP038` documentation outdated wrt. fix safety"
type: issue
state: open
author: VeckoTheGecko
labels:
  - documentation
assignees: []
created_at: 2024-08-22T09:44:59Z
updated_at: 2025-05-07T14:04:51Z
url: https://github.com/astral-sh/ruff/issues/13051
synced_at: 2026-01-12T15:54:52Z
```

# `UP038` documentation outdated wrt. fix safety

---

_@VeckoTheGecko_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


[Docs mention "Fix is always available."](https://docs.astral.sh/ruff/rules/non-pep604-isinstance/) but it is flagged as unsafe in the code
```
ruff --version
ruff 0.6.1
```

### Code
with `isinstance(2, (int,float))` I get

```
test.py:1:1: UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
  |
1 | isinstance(2, (int, float))
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^ UP038
  |
  = help: Convert to `X | Y`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

Related to #2923


---

_Comment by @MichaReiser on 2024-08-22 10:39_

Thanks for opening this issue. I can see how this is confusing. I was actually just about to change the rule metadata but then noticed that, maybe I shouldn't.

The documentation's fix availability doesn't consider the fix's safety today, which makes sense because the fix is always available when using `--unsafe-fixes`. But I can see how this isn't very clear, especially with the CLI message that explicitly states that the fix is unavailable. 

@zanieb what's your take on fix availability and fix safety?

---

_Comment by @VeckoTheGecko on 2024-08-22 15:05_

I see, so availability != safety (where I assumed it did). If safety was also mentioned in the doc that would have cleared it up (e.g., Fix is always available but not always safe)

---

_Label `documentation` added by @MichaReiser on 2024-08-22 15:22_

---

_Comment by @dhruvmanila on 2024-08-23 05:24_

Yeah, I think it might be useful to add some reference to fix safety in the rule header. It might also be useful to either provide a different icon or a different color to the fix icon in the rules table (https://docs.astral.sh/ruff/rules/#legend).

---

_Comment by @VeckoTheGecko on 2025-05-07 08:36_

The deprecation of this rule in #16681 means this can be closed

---

_Closed by @VeckoTheGecko on 2025-05-07 08:36_

---

_Comment by @VeckoTheGecko on 2025-05-07 08:39_

OH. I just realised that the title wasn't accurate anymore after the discussion, so maybe this was closed pre-maturely. @dhruvmanila is your comment https://github.com/astral-sh/ruff/issues/13051#issuecomment-2306315853 still relevant here do you think? And if so, would you mind re-opening (I don't have the perms) and updating the title?

---

_Reopened by @ntBre on 2025-05-07 14:04_

---
