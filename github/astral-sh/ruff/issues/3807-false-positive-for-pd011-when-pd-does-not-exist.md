---
number: 3807
title: "False positive for `PD011` when `pd` does not exist in module"
type: issue
state: closed
author: danparizher
labels:
  - type-inference
assignees: []
created_at: 2023-03-30T04:25:59Z
updated_at: 2024-10-07T07:39:34Z
url: https://github.com/astral-sh/ruff/issues/3807
synced_at: 2026-01-07T13:12:14-06:00
---

# False positive for `PD011` when `pd` does not exist in module

---

_Issue opened by @danparizher on 2023-03-30 04:25_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Code Snippet:
```py
async def select_callback(
    self,
    select: discord.ui.Select,
    interaction: discord.Interaction,
) -> None:  # the function called when the user is done selecting options
    value = select.values[0]
    if value == "current_value":
        # do nothing
        await interaction.response.defer()
        return
```
`PD011` comes up on the line `value = select.values[0]`, but pd doesn't even exist in this module.


---

_Label `type-inference` added by @charliermarsh on 2023-03-30 13:09_

---

_Comment by @charliermarsh on 2023-03-30 13:09_

Unfortunately this is hard to prevent. We can't rely on the presence of a `pd` import, since you can use Pandas in a given module without importing it (e.g., imagine you call a function, defined in another file, that returns a DataFrame, and call `.values` on that DataFrame). 

---

_Comment by @gustavi on 2023-06-09 09:53_

Same issue here with Django on https://github.com/django/django/blob/4.2/django/db/models/enums.py#L55.

I got the problme with `ACL.Type.values` where `ACL` is the Django model and `Type` a `TextChoices`.

---

_Comment by @MichaReiser on 2023-06-09 12:23_

One idea that we could support is to detect the frameworks in a project (or configure them manually for the time being), and then enable/disable the appropriate rules based on that (or use that knowledge for preciser inference)

---

_Comment by @souliane on 2024-10-02 07:19_

I also get a false positive with this minimal example:
```python
class Dummy:
    values: str


x = Dummy().values
```
```shell
❯ ruff --version         
ruff 0.6.8

❯ ruff check test_ruff.py
test_ruff.py:5:5: PD011 Use `.to_numpy()` instead of `.values`
  |
5 | x = Dummy().values
  |     ^^^^^^^^^^^^^^ PD011
  |
```
Given the simplicity of this code, I would expect ruff to rule out the possibility of `Dummy` being a pandas dataframe, but I understand that whether the code is simple or not does not impact the result. Or is this a similar but actually a different issue?

---

_Comment by @MichaReiser on 2024-10-07 07:39_

This is related to https://github.com/astral-sh/ruff/issues/6432. I'll merge the two issues

---

_Closed by @MichaReiser on 2024-10-07 07:39_

---

_Referenced in [astral-sh/ruff#14671](../../astral-sh/ruff/pulls/14671.md) on 2024-11-29 07:33_

---
