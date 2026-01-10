---
number: 8705
title: Use the indent-width setting for E111 and E114
type: issue
state: open
author: NeilGirdhar
labels:
  - rule
  - configuration
  - help wanted
assignees: []
created_at: 2023-11-15T22:37:17Z
updated_at: 2025-05-06T01:34:43Z
url: https://github.com/astral-sh/ruff/issues/8705
synced_at: 2026-01-10T01:22:48Z
---

# Use the indent-width setting for E111 and E114

---

_Issue opened by @NeilGirdhar on 2023-11-15 22:37_

[E111](https://docs.astral.sh/ruff/rules/indentation-with-invalid-multiple/) should probably use the indent-width setting?

---

_Renamed from "Use the indent-width setting for E111" to "Use the indent-width setting for E111 and E114" by @NeilGirdhar on 2023-11-15 22:38_

---

_Comment by @zanieb on 2023-11-15 23:10_

Hm it looks like these intentionally _do not_ use this setting since there are notes about conflict with the formatter and they explicitly mention using a width of "4" in the documentation. I'm not sure I have the context to approve changing this. cc @charliermarsh / @MichaReiser 

---

_Comment by @charliermarsh on 2023-11-15 23:10_

I believe this is intentional since the pycodestyle rules are definitionally intended to adhere to PEP 8.

---

_Label `rule` added by @zanieb on 2023-11-15 23:10_

---

_Label `configuration` added by @zanieb on 2023-11-15 23:10_

---

_Comment by @zanieb on 2023-11-15 23:13_

Interestingly though, pycodestyle lets you configure the size.

---

_Comment by @charliermarsh on 2023-11-15 23:14_

Oh, it does?

---

_Comment by @charliermarsh on 2023-11-15 23:15_

Interesting.

---

_Referenced in [astral-sh/ruff#8708](../../astral-sh/ruff/pulls/8708.md) on 2023-11-15 23:18_

---

_Comment by @zanieb on 2023-11-15 23:19_

Here's a start https://github.com/astral-sh/ruff/pull/8708 — let me know if you're interested in writing the tests.

---

_Label `help wanted` added by @zanieb on 2023-11-16 20:08_

---

_Comment by @charliermarsh on 2024-01-28 03:34_

I thought I merged something related to this recently, but I can't find it...

---

_Comment by @charliermarsh on 2024-01-28 03:35_

Oh, I was thinking of https://github.com/astral-sh/ruff/pull/9506.

---

_Comment by @asmaier on 2024-04-26 15:54_

There is also a documentation bug at https://docs.astral.sh/ruff/rules/#pycodestyle-e-w . It says

    E111 Indentation is not a multiple of {indent_size}
    ...
    E114 Indentation is not a multiple of {indent_size} (comment)

However in settings the variable is called `indent-width`, see https://docs.astral.sh/ruff/settings/#indent-width

---

_Comment by @dhruvmanila on 2024-04-30 08:39_

> There is also a documentation bug at [docs.astral.sh/ruff/rules/#pycodestyle-e-w](https://docs.astral.sh/ruff/rules/#pycodestyle-e-w) . It says
> 
> ```
> E111 Indentation is not a multiple of {indent_size}
> ...
> E114 Indentation is not a multiple of {indent_size} (comment)
> ```
> 
> However in settings the variable is called `indent-width`, see [docs.astral.sh/ruff/settings/#indent-width](https://docs.astral.sh/ruff/settings/#indent-width)


Ah, that's because it's the name of the variable used internally: https://github.com/astral-sh/ruff/blob/c391c8b6cbbf798970f8a0ab4cfd3250eff38f1f/crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/indentation.rs#L76-L87

I think we should just rename that to `indent_width`. Do you want to open a PR for that? @asmaier 


---

_Comment by @charliermarsh on 2024-04-30 16:08_

Yeah let's just rename that variable.

---

_Referenced in [astral-sh/ruff#11230](../../astral-sh/ruff/pulls/11230.md) on 2024-05-01 11:56_

---

_Comment by @sixshotx on 2025-05-06 01:34_

Looks like ruff still does not take `indent-width` for E111 and E114? I am using `0.9.1`.

```
warning: The `format.indent-width` option with a value other than 4 is incompatible with `E111` and `E114`. We recommend disabling these rules when using the formatter, which enforces a consistent indentation width. Alternatively, set the `format.indent-width` option to `4`.
```

---

_Referenced in [commaai/opendbc#2621](../../commaai/opendbc/pulls/2621.md) on 2025-08-04 22:20_

---
