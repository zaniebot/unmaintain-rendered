---
number: 8383
title: Consider non-whitespace split points for line-too-long exemptions
type: issue
state: open
author: alanwilter
labels:
  - bug
assignees: []
created_at: 2023-10-31T17:52:10Z
updated_at: 2023-11-03T03:57:12Z
url: https://github.com/astral-sh/ruff/issues/8383
synced_at: 2026-01-07T13:12:15-06:00
---

# Consider non-whitespace split points for line-too-long exemptions

---

_Issue opened by @alanwilter on 2023-10-31 17:52_

I have this line of code:

```python
        if not opt.save_model_path:
            opt.save_model_path = os.path.abspath(
                f"models_out/FineTuned_{os.path.basename(opt.model_path).split('.')[0]}_{opt.feature_name}_{opt.loss}_{today_date_string}.pth"
            )
            print(f"Model name is automatically set to {opt.save_model_path}")
```

The offending line has 143 chars and my `pyproject.toml` is:

```toml
[tool.ruff]
target-version = "py39"
line-length = 120
extend-select = [
    "B",   # flake8-bugbear
    "C4",  # flake8-comprehensions
    "ERA", # flake8-eradicate/eradicate
    "I",   # isort
    "N",   # pep8-naming
    # "NPY", # numpy
    "PIE",  # flake8-pie
    "PGH",  # pygrep
    "RUF",  # ruff checks
    "SIM",  # flake8-simplify
    "TCH",  # flake8-type-checking
    "TID",  # flake8-tidy-imports
    "UP",   # pyupgrade
    "E501", # check line length
]
```

But `ruff check . --select E501  --line-length=120` does not pick it!
I understand that this line:

```python
f"models_out/FineTuned_{os.path.basename(opt.model_path).split('.')[0]}_{opt.feature_name}_{opt.loss}_{today_date_string}.pth"
```

cannot be automatically formatted but the others linters will spot them. 
Also, if I comment the line like:
`# hi f"models_out/FineTuned_{os.pa...` and run `ruff check . --select E501  --line-length=120` I get this:

```
isam/main.py:71:121: E501 Line too long (144 > 120)
```

So it seems like a bug for me. Because I truly want anything going beyond 120 chars limit so I can fix manually if needed.



---

_Comment by @T-256 on 2023-10-31 21:12_

```py
#LINE_WITH_89_CHARACTERSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSS
```
Also tried this with `ruff check test.py --select E501`, no violation reported!
After adding a whitespace after `#`, it starts reporting.

---

_Comment by @charliermarsh on 2023-10-31 21:16_

We don't enforce E501 on lines that consist of a single token -- this is mentioned in the rule documentation [here](https://docs.astral.sh/ruff/rules/line-too-long/). The comment is actually a decent example of why: sometimes, there just isn't a way to split a line, and so enforcing the lint rule isn't' really helpful.

I suspect we could do a better job with the f-string case, since there actually are some reasonable split points in that code.

---

_Label `question` added by @charliermarsh on 2023-10-31 21:16_

---

_Comment by @alanwilter on 2023-10-31 21:27_

But I'd still like to be warned about long lines.

```bash
flake8 test.py
test.py:2:80: E501 line too long (126 > 79 characters)
```
One of the reason I move to `ruff` was to get rid of `flake8`. It should be enforced. There's a pragma in `flake8` to ignore long lines if you really need it. 

---

_Comment by @charliermarsh on 2023-10-31 21:44_

We could consider adding a strict mode for this rule that ignores all the stated exemptions. But I think it would need to be all-or-nothing (so, e.g., when that setting is enabled, we'd also flag URLs that exceed the line length limit, which are allowed right now).

---

_Label `question` removed by @charliermarsh on 2023-10-31 21:44_

---

_Label `configuration` added by @charliermarsh on 2023-10-31 21:44_

---

_Label `needs-decision` added by @charliermarsh on 2023-10-31 21:44_

---

_Comment by @T-256 on 2023-10-31 21:46_

> 1. Ignores lines that consist of a single "word" (i.e., without any whitespace between its characters).

I agree with it, but whitespace is not only word separator. In mentioned example in description, I see +10 words.
IMO something like `str.isidentifier()` check in python could be happened instead.

---

_Comment by @alanwilter on 2023-10-31 21:51_

Reading [E501](https://docs.astral.sh/ruff/rules/line-too-long/) and I agree with the 3 exceptions. I just don't get why my original example is an exception.

---

_Comment by @charliermarsh on 2023-10-31 21:54_

@alanwilter - It's because there's no whitespace anywhere on the line, so Ruff doesn't think it's splittable. I think we can do a better job of figuring out whether the line is splittable though. (That exemption wasn't designed for cases like the one in your example, I agree that it's not quite right.)

---

_Comment by @MichaReiser on 2023-10-31 21:56_

> Reading [E501](https://docs.astral.sh/ruff/rules/line-too-long/) and I agree with the 3 exceptions. I just don't get why my original example is an exception.

It's because the line contains no whitespace (not justifying that it is correct, but it is why it the reason why the line isn't flagged):

> Ignores lines that consist of a single "word" (i.e., without any whitespace between its characters).

---

_Comment by @alanwilter on 2023-10-31 22:08_

> > 1. Ignores lines that consist of a single "word" (i.e., without any whitespace between its characters).
> 
> I agree with it, but whitespace is not only word separator. In mentioned example in description, I see +10 words. IMO something like `str.isidentifier()` check in python could be happened instead.

Ah, ok, thanks @charliermarsh. So now I agree with @T-256. IIRC, `ruff` knows about commented python code lines ([ERA001](https://docs.astral.sh/ruff/rules/commented-out-code/)), so it should know when a long line is a python code too. 

---

_Comment by @T-256 on 2023-10-31 22:24_

> `ruff` knows about commented python code lines ([ERA001](https://docs.astral.sh/ruff/rules/commented-out-code/)), so it should know when a long line is a python code too.

This check is unnecessary for this rule.


https://github.com/astral-sh/ruff/blob/1642f4dbd9d4ff04e50d08b88eebbbef6e4cc077/crates/ruff_linter/src/rules/pycodestyle/overlong.rs#L55-L59
Instead here should split by word separators instead of only whitespace.
Sample word separators got from default VSCode settings:
```
`~!@#$%^&*()-=+[{]}\|;:'",.<>/?
```


---

_Comment by @charliermarsh on 2023-10-31 23:30_

I'm open to something like that, though we may need to see how it affects existing projects (via our ecosystem checks, which run automatically on every PR).

---

_Label `configuration` removed by @charliermarsh on 2023-11-03 03:57_

---

_Label `needs-decision` removed by @charliermarsh on 2023-11-03 03:57_

---

_Label `bug` added by @charliermarsh on 2023-11-03 03:57_

---

_Renamed from "Line length check/format does not work in a specific case" to "Consider non-whitespace split points for line-too-long exemptions" by @charliermarsh on 2023-11-03 03:57_

---

_Referenced in [astral-sh/ruff#8922](../../astral-sh/ruff/issues/8922.md) on 2023-11-30 14:27_

---

_Referenced in [astral-sh/ruff#8939](../../astral-sh/ruff/pulls/8939.md) on 2023-12-01 03:32_

---

_Referenced in [astral-sh/ruff#11432](../../astral-sh/ruff/issues/11432.md) on 2024-05-15 19:07_

---

_Referenced in [astral-sh/ruff#15236](../../astral-sh/ruff/issues/15236.md) on 2025-01-03 08:17_

---
