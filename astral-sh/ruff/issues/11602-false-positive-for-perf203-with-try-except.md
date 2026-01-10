```yaml
number: 11602
title: false positive for PERF203 with try-except-continue-else
type: issue
state: closed
author: trim21
labels:
  - rule
assignees: []
created_at: 2024-05-29T17:41:14Z
updated_at: 2024-09-16T23:39:37Z
url: https://github.com/astral-sh/ruff/issues/11602
synced_at: 2026-01-10T11:09:53Z
```

# false positive for PERF203 with try-except-continue-else

---

_Issue opened by @trim21 on 2024-05-29 17:41_

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

ruff 0.6.5 raise false positive for try-except in a look with `else`

```python
import io
import unicodedata


def __clean_rss_invalid_char(s: str):
    with io.StringIO() as f:
        for c in s:
            try:
                unicodedata.name(c)
            except ValueError:
                continue
            else:
                f.write(c)

        return f.getvalue()
```

previous issue: https://github.com/astral-sh/ruff/issues/6535

this looks like https://github.com/astral-sh/ruff/issues/6535 but it still exists then targeting py38

https://play.ruff.rs/eadb6e43-5f78-4bcd-93f3-2d409bdbf4c8

---

_Renamed from "false positive for PERF203 " to "false positive for PERF203 with try-except-else" by @trim21 on 2024-05-29 17:41_

---

_Comment by @trim21 on 2024-05-29 17:45_

Oh, sorry. looks like I'm actually using ruff 0.4.5.

---

_Closed by @trim21 on 2024-05-29 17:45_

---

_Reopened by @trim21 on 2024-09-16 01:39_

---

_Renamed from "false positive for PERF203 with try-except-else" to "false positive for PERF203 with try-except-continue-else" by @trim21 on 2024-09-16 04:20_

---

_Comment by @MichaReiser on 2024-09-16 08:50_

Looking at the code, the reason why the above is flagged and wasn't addressed by #6535 is that the rule only checks for `continue` and `break` statements inside the `try` body but not in the except handlers

https://github.com/astral-sh/ruff/blob/f9d818967075df1a1f4d15760c0244f941d77e53/crates/ruff_linter/src/rules/perflint/rules/try_except_in_loop.rs#L106-L108

The rule only applies to Python 310 or older because

https://github.com/astral-sh/ruff/blob/f9d818967075df1a1f4d15760c0244f941d77e53/crates/ruff_linter/src/rules/perflint/rules/try_except_in_loop.rs#L25-L30

---

_Comment by @MichaReiser on 2024-09-16 09:00_

I still think the rule should flag the above pattern because whether there's a better way to write the above loop depends on the `unicodedata` API. For example, I think the following could work:

```python
for c in s:
      name = unicodedata.name("a", None)

      if name is None:
          continue
      else:
          f.write(c)
```

which eliminates the need for try

---

_Label `rule` added by @MichaReiser on 2024-09-16 09:01_

---

_Comment by @trim21 on 2024-09-16 15:13_

> I still think the rule should flag the above pattern because whether there's a better way to write the above loop depends on the `unicodedata` API. For example, I think the following could work:
> 
> ```python
> for c in s:
>       name = unicodedata.name("a", None)
> 
>       if name is None:
>           continue
>       else:
>           f.write(c)
> ```
> 
> which eliminates the need for try

I don't think ruff understand `unicodedata.name`, it flag this didn't base on "whether there's a better way".

because ruff still flag this, and I don't think there is a better way to write this:

```python
import requests


def check_url_live(urls: str):
    for url in urls:
        try:
            requests.get(url, timeout=5)
        except ConnectionRefusedError:
            continue
        else:
            print(url)

```

---

_Comment by @trim21 on 2024-09-16 15:28_

if you think it should flag this, then there is another problem: it doesn't flag this:

```python
import unicodedata


def check_url_live(s: str):
    for c in s:
        try:
            unicodedata.name(c)
        except ValueError:
            continue

        print(c)
```

---

_Comment by @MichaReiser on 2024-09-16 15:47_

> because ruff still flag this, and I don't think there is a better way to write this:

Yeah, there's probably not a better way to write this and using a noqa comment here seems appropriate. 

> if you think it should flag this, then there is another problem: it doesn't flag this:

I would have to double check the upstream rule to understand if this is a bug but the rule currently only flags cases where the `try` statement is the only statement in the loop body. 

---

_Comment by @MichaReiser on 2024-09-16 15:50_

Going through the history. Ignoring multi-statement loops is intentional, see https://github.com/astral-sh/ruff/pull/6145

Using noqa comment seems the right solution to deal with cases where using `try...except` is the only solution because the API doesn't provide an alternative way of testing the input. Making this decision isn't something that ruff can do (even with better inference). 

---

_Closed by @MichaReiser on 2024-09-16 15:50_

---

_Comment by @trim21 on 2024-09-16 23:36_

> Going through the history. Ignoring multi-statement loops is intentional, see #6145
> 
> Using noqa comment seems the right solution to deal with cases where using `try...except` is the only solution because the API doesn't provide an alternative way of testing the input. Making this decision isn't something that ruff can do (even with better inference).

I think we are actully talking about 2 topics here... 

Ignoring multi-statement loops is fine to me, and this issue is not about it should or should not ignore multi-statement loops. the "false positive"  is, it didn't ignore single `try-except(continue)-else` statments like multi-statement loops

---
