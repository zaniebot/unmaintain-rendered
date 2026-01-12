```yaml
number: 7716
title: "Request: `select=[\"AUTOFIXABLE\"]` 🚀 "
type: issue
state: closed
author: jamesbraza
labels: []
assignees: []
created_at: 2023-09-29T17:11:55Z
updated_at: 2025-08-08T20:07:52Z
url: https://github.com/astral-sh/ruff/issues/7716
synced_at: 2026-01-12T15:54:47Z
```

# Request: `select=["AUTOFIXABLE"]` 🚀 

---

_@jamesbraza_

I think `ruff` has some excellent opt-in rules enabled by `select=["ALL"]`.  However, for large new projects, I either have to:

- Fix all the non-autofix errors (hundreds to thousands)
- Use the `--add-noqa` flag

I would like to request a halfway `select=["ALL"]` called `select=["AUTOFIXABLE"]`, where basically one can turn on all rules that have autofixes implemented.

To accomplish this right now, one can either:

- Use `select=["ALL"]`, writing out a blocklist `ignore = [...]` for non-autofix rules
- Use `select=["I001", ...]`, writing out an allowlist of autofixable rules
    - What I did here and it took hours: https://github.com/jerryjliu/llama_index/pull/7889

Also, there can be other wordings besides `"AUTOFIXABLE"`, maybe `"FIXABLE"` or `"AUTOFIX"`

---

_Comment by @zanieb on 2023-09-29 17:18_

Whether or not a rule violation can be fixed is not constant — some rules can only be fixed in certain contexts.

I'm not quite sure I understand why you want to be able to select all the fixable rules and ignore the others. It sounds like perhaps you're just looking for `ruff check --select ALL --fix-only`?

---

_Comment by @zanieb on 2023-09-29 17:19_

Would your use-case also be solved with a `ruff fix` command that fixes all fixable, selected lint rules and doesn't report additional violations?

---

_Comment by @jamesbraza on 2023-09-29 17:34_

Yeah you're understanding my request, thanks!

> some rules can only be fixed in certain contexts.

Yeah, this is the reason I didn't include certain autofixable rules (e.g. `ISC001`, `SIM102`) in that linked PR, I only included autofixable rules that universally fixed all errors.

---

`ruff check --select ALL --fix-only` works for the initial invocation, wow that is nice!

However, `--fix-only` won't work for a `pre-commit` integration for all time.  Let's say someone decides to add a rule that doesn't have autofixes. I believe `--fix-only` would filter out errors that didn't have autofixes going forwards

What do you think?

---

_Comment by @charliermarsh on 2024-01-08 03:52_

> However, --fix-only won't work for a pre-commit integration for all time. Let's say someone decides to add a rule that doesn't have autofixes. I believe --fix-only would filter out errors that didn't have autofixes going forwards

Can you say a bit more about this?


---

_Comment by @jamesbraza on 2024-01-08 07:16_

Ha it's funny, re-reading my comment that you're asking for clarification, it took me a minute to re-process what I meant. It was a bit vague, and I don't think the `pre-commit` part mattered.

Let's say we start with this:

```toml
fix-only = true
select = [
    "B009",  # Has autofix
]
```

Then we move to this:

```toml
fix-only = true
select = [
    "B008",  # Has no autofix
    "B009",  # Has autofix
]
```

The added `B008` will effectively be silenced by `fix-only`. I guess my thoughts were:

- Yes `--fix-only` is what this request was asking for, so I will close this out
- But it creates a possibility for unexpectedly silenced rules

Given though users opt-in to `fix-only`, I think unexpectedly silencing rules would be the fault of the user. Maybe `ruff` could log a warning if `fix-only` is silencing a rule without autofixes, but I think that is quite tricky, especially since `ruff` can inherit configs using `extend`. Writing this out made me realize I should just close this out 😅 

---

_Closed by @jamesbraza on 2024-01-08 07:16_

---

_Comment by @mateenkasim on 2025-08-08 20:07_

I know this is old, but I would love for it to be reconsidered. I'm not running Ruff manually, but rather having VSCode automatically run it. I'm interested in only enabling the rules that can be auto-fixed on save. I don't want the other ones highlighted.

If there's another way to do that, happy to use that instead. Thanks for all the work on Ruff!

---
