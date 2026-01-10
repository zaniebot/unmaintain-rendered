```yaml
number: 9870
title: Different length sort behavior against isort
type: issue
state: open
author: yanyongyu
labels:
  - isort
  - needs-decision
assignees: []
created_at: 2024-02-07T02:43:59Z
updated_at: 2025-12-05T21:41:32Z
url: https://github.com/astral-sh/ruff/issues/9870
synced_at: 2026-01-10T11:09:52Z
```

# Different length sort behavior against isort

---

_Issue opened by @yanyongyu on 2024-02-07 02:43_

I'm trying to migrate isort and black to ruff but encountered some problems when using ruff's isort configs.

Ruff version 0.2.1.

Suppose I have a first-party package named `package` and the test code is:

```python
from package.utils import a
from package.func import function
```

with the ruff config:

```toml
[tool.ruff.lint]
select = ["I"]

[tool.ruff.lint.isort]
length-sort = true
force-sort-within-sections = true
```

ruff will report import order error and fix it into:

```python
from package.func import function
from package.utils import a
```

but isort sorts it as the origin test code given above.

I make one more test, renaming the `package.func` to `package.function`:

```python
from package.utils import a
from package.function import function
```

this time ruff does not report any error. It seems ruff sort the import order by module name length instead of import expression length?

---

_Comment by @charliermarsh on 2024-02-07 03:12_

Can you clarify for me -- if I run `isort` over:

```python
from package.utils import a
from package.func import function
```

With `length_sort = true`, it rewrites to:

```python
from package.func import function
from package.utils import a
```

In my testing, Ruff has the same behavior. But it seems like you're seeing different behavior with isort?


---

_Label `question` added by @charliermarsh on 2024-02-07 03:12_

---

_Comment by @yanyongyu on 2024-02-07 03:14_

i sort the code with: (isort 5.13.2)

```bash
isort --profile black --length-sort --force-sort-within-sections package/__init__.py
```

---

_Comment by @yanyongyu on 2024-02-07 03:19_

Or use the pyproject.toml to config isort:

```toml
[tool.ruff.lint]
select = ["I"]

[tool.ruff.lint.isort]
length-sort = true
force-sort-within-sections = true

[tool.isort]
profile = "black"
length_sort = true
force_sort_within_sections = true
```

then sort the imports with `isort .`

the output code is:

```python
from package.utils import a
from package.func import function
```

---

_Comment by @charliermarsh on 2024-02-07 03:19_

It this an isort bug? Removing `--force-sort-within-sections` shows the same behavior as Ruff, but I don't see why `--force-sort-within-sections ` should have an effect on the sort in this case (both imports are in the same section).

---

_Comment by @yanyongyu on 2024-02-07 03:22_

> Isort docs:
> Length Sort: Sort imports by their string length.
> Force Sort Within Sections: Instead, sort the imports by module, independent of import style.

I'm not sure which behavior is correct 😂 , but isort keeps this behavior for years.

---

_Comment by @charliermarsh on 2024-02-07 03:27_

Haha :)

I looked through the isort source. It looks like if `--force-sort-within-sections` is set, it sorts all lines by their exact length. If it's not set, it sorts by the length of the module, then sorts each member by length. So the behavior is totally different when that flag is set. I find it confusing... But if we don't support it, we may want to document it at least.


---

_Label `question` removed by @charliermarsh on 2024-02-07 03:27_

---

_Label `isort` added by @charliermarsh on 2024-02-07 03:27_

---

_Label `needs-decision` added by @charliermarsh on 2024-02-07 03:27_

---

_Comment by @tomprimozic on 2024-02-26 23:20_

I'd also want a "sort imports by line length exactly" with no extra exceptions. `isort` does (almost) support that (except some edge cases / bugs) using `length_sort` and `force_sort_within_sections` options, it would be great if `ruff` did as well.

For the time being, I have to use 2 tools :/

---

_Comment by @yanyongyu on 2024-04-15 08:16_

@charliermarsh Is there any decision about this issue? Currently, i also have to use 2 tools (isort + ruff) to lint and format the code. i hope this feature can be part of this excellent project. :pray:

---

_Comment by @rainzee on 2024-04-19 16:55_

This is really the last hurdle for me to fully use ruff, now I have to use both tools, hopefully some progress will be made, please!🙏

---

_Comment by @forchannot on 2024-06-14 12:58_

any progress on this issue?

---

_Comment by @KimigaiiWuyi on 2025-12-05 19:14_

any progress?

Due to differences in sorting behavior between `ruff` and `isort`, the project code becomes messy when using them together. I would like to make this a feature request to keep `ruff`'s sorting consistent with `isort`

#14136 

---

_Comment by @MichaReiser on 2025-12-05 21:41_

We don't recommend using isort with ruff. Use one or the other

---
