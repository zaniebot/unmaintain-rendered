```yaml
number: 13018
title: UP031 does not detect cases where percent operator is used on an f-string
type: issue
state: open
author: richardxia
labels:
  - bug
assignees: []
created_at: 2024-08-20T22:18:42Z
updated_at: 2025-07-10T00:08:31Z
url: https://github.com/astral-sh/ruff/issues/13018
synced_at: 2026-01-10T11:09:55Z
```

# UP031 does not detect cases where percent operator is used on an f-string

---

_Issue opened by @richardxia on 2024-08-20 22:18_

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

This is admittedly a bit of a weird case, but I saw it in the wild and was surprised that Ruff wasn't flagging it as a lint error. With the `UP` ruleset enabled, Ruff isn't detecting the case where the `%` formatting operator is used on an f-string. Here's an example:

```python
def percent_formatting(x: str) -> str:
    return "%s" % x


def percent_formatting_and_f_string(x: str, y: str) -> str:
    return f"{x} %s" % y
```

Ruff will correctly flag the first function as having an issue, but the second one it silently ignores. I've created a playground link for this: https://play.ruff.rs/d0eec036-39bb-4db1-b55c-2dde7babaf7d

What I'd ideally expect is for Ruff to flag the second case as an issue and to suggest that the argument to the `%` be folded into the f-string, as `f"{x} {y}"`. Even if it can't suggest that specific fix, I think it would good to at least report that there is an error and that the author shouldn't be mixing formatting types like that.

---

_Label `needs-triage` added by @MichaReiser on 2024-08-22 13:18_

---

_Label `needs-triage` removed by @MichaReiser on 2024-11-26 06:57_

---

_Comment by @MichaReiser on 2024-11-26 06:57_

That makes sense. Thanks for reporting this

---

_Label `bug` added by @MichaReiser on 2024-11-26 06:57_

---

_Comment by @dhruvmanila on 2024-11-27 10:53_

I don't think I would consider this as a bug because we need to expand the scope of the rule to cover f-strings as currently it only checks for string literals.

---

_Comment by @MichaReiser on 2024-11-27 11:52_

I'm not very opinionated whether it's a bug or not. My take here is that, as a user, I would find it surprising that UP031 doesn't flag f-strings. But I could also see an argument that this could be its own rule (although that's another case where I think we're leaning too much towards fine granular rules)

---

_Comment by @Avasam on 2025-04-28 21:51_

Whether a different rule or an expansion of the existing one, I'm +1 on wanting this. There's an argument to be made that splitting this off into its own rule allows for more granularity for the users. (`UP031` is already very hard to add to existing projects)

Regarding autofixes, I think the same as current `UP031` autofixes rules apply? Where Ruff needs to know the exact number of params to pass to the string (whether it's a tuple, a literal tuple, a single item, ...)

In OP's `percent_formatting_and_f_string` example, if  `x = "%s"` then it would already be failing.

---

_Comment by @MeGaGiGaGon on 2025-07-10 00:08_

One additional wrinkle with this is that f-strings are evaluated before the `%` formatting, so you could have a situation where an f-string replacement is inserted to add percent formats, ie
```py
def example(insert, value):
    print(f"{insert}" % value)
example("%s", 1)
example("%s %s", (1, 2))
```
I'm not sure where you would run into this in normal code, but the fact it can happen makes it hard to say any f-string with a replacement isn't going to add/remove formats. This sounds to me like it's the same reason why `UP031` doesn't lint on complex expressions like concatenations, ie `("%s" + x) % y` https://play.ruff.rs/af9fdd40-679d-4844-b68a-21edd5562689

---
