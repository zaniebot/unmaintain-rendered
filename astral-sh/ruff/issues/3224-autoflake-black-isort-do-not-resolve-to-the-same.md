---
number: 3224
title: "`autoflake`/`black`/`isort` do not resolve to the same file"
type: issue
state: closed
author: dycw
labels:
  - question
assignees: []
created_at: 2023-02-25T13:02:49Z
updated_at: 2023-03-03T08:01:27Z
url: https://github.com/astral-sh/ruff/issues/3224
synced_at: 2026-01-10T01:22:41Z
---

# `autoflake`/`black`/`isort` do not resolve to the same file

---

_Issue opened by @dycw on 2023-02-25 13:02_

Suppose a file is stable under `autoflake`, `black` and `isort`. If an import is added and then later unused and removed via `autoflake`, one might presume `black` and `isort` would resolve back to the same file. This certainly introduces potentially many diffs.

### Minimal example

Start with an empty directory with `foo.py` and no config files of any sort:

```python
# foo.py
from package import item1, item2, item3, item4, item5, item6, item7, item8, item9

a = item1, item2, item3, item4, item5, item6, item7, item8, item9
```

This is stable under `autoflake`, `black` and `isort`:

```bash
❯ black foo.py && ruff foo.py --select=F401,I001 --fix
All done! ✨ 🍰 ✨
1 file left unchanged.
```

Now add an additional import to breach `black`'s default line-length:

```python
# foo.py
from package import item1, item2, item3, item4, item5, item6, item7, item8, item9
from package import item10

a = item1, item2, item3, item4, item5, item6, item7, item8, item9
a = item10
```

This is stable after two runs:

```bash
❯ black foo.py && ruff foo.py --select=F401,I001 --fix
All done! ✨ 🍰 ✨
1 file left unchanged.
Found 1 error (1 fixed, 0 remaining).

❯ black foo.py && ruff foo.py --select=F401,I001 --fix
All done! ✨ 🍰 ✨
1 file left unchanged.
```

with the end result being:

```python
# foo.py
from package import (
    item1,
    item2,
    item3,
    item4,
    item5,
    item6,
    item7,
    item8,
    item9,
    item10,
)

a = item1, item2, item3, item4, item5, item6, item7, item8, item9
a = item10
```

Now remove the usage of `item10`, i.e., the last line. `autoflake` will remove the import of `item10`, with the end result (again stable after two runs):

```python
# foo.py
from package import (
    item1,
    item2,
    item3,
    item4,
    item5,
    item6,
    item7,
    item8,
    item9,
)

a = item1, item2, item3, item4, item5, item6, item7, item8, item9
```

However, this is unlikely the desired result. Removing successive imports can even leave you with:

```python
# foo.py
from package import (
    item1,
)

a = item1
```

This is most certainly not the desired result.

P.S. Workarounds exist: e.g., run `isort` once with `force-single-line` and then you can disable it again to recover the very original stable state.

---

_Comment by @charliermarsh on 2023-02-25 16:46_

The thing is, I think this is consistent with the isort and black's behaviors: they always add the trailing comma when expanding, and they respect trailing comma when contracting, so it's not possible to revert back to a state like `from package import item1` once you've expanded an import. I don't see how we could support this without _not_ respecting trailing commas.


---

_Label `question` added by @charliermarsh on 2023-02-25 16:46_

---

_Comment by @dycw on 2023-02-26 01:10_

Hi @charliermarsh , I understand, however I feel there is a case here because I strongly subscribe to `black`'s own philosophy of minimizing diffs. To that end, this is the workaround I mentioned:

```python
# foo.py
from package import (
    item1,
)

a = item1
```

If we add `force-single-line`

```bash
echo '[tool.ruff.isort]\nforce-single-line = true' > pyproject.toml
```

and then re-run:

```bash
❯ black foo.py && ruff foo.py --select=F401,I001 --fix
All done! ✨ 🍰 ✨
1 file left unchanged.
Found 1 error (1 fixed, 0 remaining).
```

we get the required behavior:

```python
# foo.py
from package import item1

a = item1
```

Of course, I'd then essentially remove 

```bash
 ❯ rm pyproject.toml
```

to recover the original settings.

---

_Comment by @tmke8 on 2023-02-28 11:22_

You can run black with `--skip-magic-trailing-comma` then it will always collapse the imports when possible.

---

_Comment by @dycw on 2023-03-03 08:01_

Thank you @tmke8 , I think this works just about perfectly.

---

_Closed by @dycw on 2023-03-03 08:01_

---
