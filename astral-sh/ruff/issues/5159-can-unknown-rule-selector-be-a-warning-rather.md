---
number: 5159
title: "Can \"Unknown rule selector\" be a warning, rather than an error?"
type: issue
state: closed
author: KyleKing
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2023-06-17T12:59:00Z
updated_at: 2025-11-20T01:40:42Z
url: https://github.com/astral-sh/ruff/issues/5159
synced_at: 2026-01-10T01:22:44Z
---

# Can "Unknown rule selector" be a warning, rather than an error?

---

_Issue opened by @KyleKing on 2023-06-17 12:59_

I ran into an issue recently when:

1. A new version of `ruff` adds a new rule that I choose to ignore (i.e. `FIX002` for `TODO` comments)
2. I update my `.ruff.toml` to ignore `FIX002` (https://github.com/KyleKing/calcipy_template/commit/5a439c640ca23f1d31bb2b094e4ae821d0bf6dae)
3. If I don't update `ruff` in `pre-commmit`, `.venv`, `pipx`, `CI`, and anywhere else I use it, I'll start getting "Unknown rule selector" errors, then will spend time yak shaving/whack-a-mole to update `ruff` to the latest version

Instead, I would prefer `ruff` to show a warning rather than raise an exception. In particular, for this use case of new `ignore` rules, older versions of ruff wouldn't have have caught the new rule anyway

---

_Label `question` added by @charliermarsh on 2023-06-17 14:44_

---

_Label `configuration` added by @charliermarsh on 2023-06-17 14:44_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:10_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:10_

---

_Comment by @adifelice-godaddy on 2023-09-04 17:52_

We are also getting similar errors:
```
error: Failed to parse `ruff.ignore.toml`: TOML parse error at line 29, column 5
29 |     "PLR1714",
   |     ^^^^^^^^^
Unknown rule selector: `PLR1714`
```
```
$ ruff --version
ruff 0.0.287
```

Is there a workaround? Thanks.

---

_Comment by @MichaReiser on 2023-09-05 06:53_

Thanks for explaining your use case. IMO, raising an error for unknown rules in rule selectors is the intended behavior because it ensures all rules are running the way you intended. However, I can see how the ignore selector is different because it doesn't make you *miss* out on lints that you wanted to have enabled.

---

_Comment by @KyleKing on 2023-09-05 11:15_

That seems reasonable. So [from the configuration](https://beta.ruff.rs/docs/configuration/#using-pyprojecttoml), maybe:

- warning:
	- `ignore`
	- `unfixable`
	- `per-file-ignores`
- error
	- `select`
	- `fixable`

---

_Comment by @MichaReiser on 2023-09-05 19:58_

I haven't tried it myself but I assume you could use [`external`](https://beta.ruff.rs/docs/settings/#external) to make ruff ignore the violation. But I don't know if that breaks e.g. no-unused-noqa 

---

_Comment by @KyleKing on 2023-09-05 20:28_

Oh cool, I didn’t know about external! I’ll start using that so I can know what flake8 rules are still used.

However, I don’t think external+noqa will help for this particular issue because of the number of noqa comments that might be needed.

The workaround right now is to pin ruff and update everywhere all at once.

---

_Closed by @KyleKing on 2025-11-20 01:40_

---
