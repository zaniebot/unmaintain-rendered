```yaml
number: 8640
title: "`FURB113`: False positive"
type: issue
state: closed
author: ofek
labels:
  - bug
assignees: []
created_at: 2023-11-12T23:30:25Z
updated_at: 2023-11-13T05:36:50Z
url: https://github.com/astral-sh/ruff/issues/8640
synced_at: 2026-01-12T15:54:48Z
```

# `FURB113`: False positive

---

_@ofek_

```
Use `output.extend(...)` instead of repeatedly calling `output.append()`
```

```python
ordered = [identifier for identifiers in ordering_data.values() for identifier in reversed(identifiers)]
output = [
    'from __future__ import annotations',
    '',
    '# fmt: off',
    'ORDERED_DISTRIBUTIONS: tuple[str, ...] = (',
]
output.extend(f'    {identifier!r},' for identifier in ordered)
output.append(')')  # error here
```

---

_Renamed from "`FURB113`: False negative" to "`FURB113`: False positive" by @ofek on 2023-11-12 23:30_

---

_Label `bug` added by @charliermarsh on 2023-11-13 00:47_

---

_Comment by @dhruvmanila on 2023-11-13 04:27_

I'm unable to reproduce this with the provided code snippet. Can you please provide the Ruff version you're running against?

---

_Comment by @ofek on 2023-11-13 04:33_

`0.1.4`

---

_Comment by @dhruvmanila on 2023-11-13 04:41_

Thanks, I'm unable to reproduce this with the following command:
```console
$ pipx run "ruff==0.1.4" check --select=FURB113 --no-cache --isolated src/FURB113.py --preview
```

Neither when using `--select=ALL`. Where are you running Ruff from? Command-line, editor, or as a pre-commit hook? Which command did you use to run Ruff if from the command-line?

---

_Comment by @charliermarsh on 2023-11-13 04:41_

Seems like there's some missing context -- I don't see it in the playground: https://play.ruff.rs/94de3bbb-5f0f-47b0-9cfa-47587fac4d4b

---

_Comment by @ofek on 2023-11-13 05:28_

```
git clone https://github.com/pypa/hatch
cd hatch
```

remove the ignore comment

https://github.com/pypa/hatch/blob/1e7f8ff29a5ca264c7a8531de70171f7184b1501/scripts/update_distributions.py#L66

then run

```
ruff check --isolated --preview --select FURB113 scripts/update_distributions.py
```

---

_Comment by @dhruvmanila on 2023-11-13 05:32_

Thanks for the context!

I think it's correct as there's an `append` call right after that. So, 
```python
    output = [
        'from __future__ import annotations',
        '',
        '# fmt: off',
        'ORDERED_DISTRIBUTIONS: tuple[str, ...] = (',
    ]
    output.extend(f'    {identifier!r},' for identifier in ordered)
    output.append(')')  # noqa: FURB113
	# `FURB113` is highlighting to merge the above and below `append` calls
    output.append('DISTRIBUTIONS: dict[str, dict[tuple[str, ...], str]] = {')
```

---

_Comment by @ofek on 2023-11-13 05:35_

I'm sorry 🤦‍♂️ my brain is fried from updating the entire code base this weekend

---

_Closed by @ofek on 2023-11-13 05:35_

---

_Comment by @dhruvmanila on 2023-11-13 05:36_

> I'm sorry 🤦‍♂️ my brain is fried from updating the entire code base this weekend

Haha, no worries. Happy to help :)

---
