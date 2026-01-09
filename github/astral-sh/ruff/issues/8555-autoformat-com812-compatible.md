---
number: 8555
title: Autoformat COM812 compatible
type: issue
state: open
author: maltevesper
labels:
  - wish
  - linter
assignees: []
created_at: 2023-11-08T11:04:57Z
updated_at: 2024-12-04T14:52:20Z
url: https://github.com/astral-sh/ruff/issues/8555
synced_at: 2026-01-07T13:12:15-06:00
---

# Autoformat COM812 compatible

---

_Issue opened by @maltevesper on 2023-11-08 11:04_

It is not immediately clear why some of the rules [documented as incompatible](https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules) with the autoformatter are incompatible with the autoformatter.
I did not understand why the autoformatter conflicts with COM812 until I ran into this example:

```
    with patch(
        "os.environ.get", return_value=None
    ):  # TODO: use a pytest fixture instead
```

This is pretty similar to Issue #6525.

I am wondering would it be possible to force each argument onto its own line with a trailing comma, *if and only if* the function is formatted as multiline anyways? (Here the trailing comment forces this formatting)
I would consider this a sane default, even if it breaks with black (see https://github.com/astral-sh/ruff/issues/6525#issuecomment-1789340668). Furthermore, I accidentally overlooked the fact that there are two arguments on the same line.
In case black priority is higher here, a configuration option would be nice (explicit is better than implicit), or one could use the fact that COM812 is enabled to change the formatter behaviour accordingly, which feels dirty.

Afterthought: I am not sure if there are other conflicts between the formatter and COM812/ the other rules. Examples would be nice, if only for the sake of knowing what would need to change to make things work. (I.e. why cant the formatter fix up ISC001 for example)


---

_Comment by @charliermarsh on 2023-11-09 02:11_

Is it fair to summarize the request here as: an option to force one-element-per-line whenever a function call or other collection is formatted over multiple lines?

---

_Label `formatter` added by @charliermarsh on 2023-11-09 02:11_

---

_Label `wish` added by @charliermarsh on 2023-11-09 02:11_

---

_Comment by @charliermarsh on 2023-11-09 02:15_

I don't know if this helps clarify, but our general goal with formatter-linter compatibility is such that running the formatter shouldn't introduce new lint violations. (E.g., COM812 is incompatible since the formatter can collapse calls that don't end in a magic trailing comma, as you pointed out above.)

---

_Comment by @maltevesper on 2023-11-09 08:54_

> Is it fair to summarize the request here as: an option to force one-element-per-line whenever a function call or other collection is formatted over multiple lines?

Sounds good to me (assuming that this is all that is needed to make COM812 compatible with the formatter).

---

_Label `formatter` removed by @MichaReiser on 2023-11-27 04:27_

---

_Label `linter` added by @MichaReiser on 2023-11-27 04:27_

---

_Referenced in [astral-sh/ruff#6525](../../astral-sh/ruff/issues/6525.md) on 2024-01-29 05:18_

---

_Comment by @pikevsten on 2024-12-04 14:39_


> @charliermarsh 
> an option to force one-element-per-line whenever a function call or other collection is formatted over multiple lines?

I was just looking for whether this specific issue exists or to avoid re-creating it.

That is:
- "force one element per line, if (and only if) they're already on multiple lines"

I'd be happy with it as a lint, so that the "magic comma" could still work to quickly reformat a short argument list into a single line.

---------

Ideally however, I think a format configuration or auto-fixable lint would be preferable for me,
and I think the rule would be more like:

- "If (and only if) all the arguments of a function definition (or of a function call)
  don't fit on the same line as the function name (given the configured line length),
  the reformatter should automatically adding a trailing comma.


------

If the Ruff authors are amenable to either approach in particular, I'd be happy to make an MR.

---
