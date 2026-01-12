```yaml
number: 3297
title: Use itertools.pairwise instead of zipping a sequence against itself
type: issue
state: closed
author: Jeremiah-England
labels:
  - rule
assignees: []
created_at: 2023-03-02T03:55:31Z
updated_at: 2023-03-17T03:50:47Z
url: https://github.com/astral-sh/ruff/issues/3297
synced_at: 2026-01-12T15:54:43Z
```

# Use itertools.pairwise instead of zipping a sequence against itself

---

_@Jeremiah-England_

Tonight I discovered [`itertools.pairwise`][0] (new in 3.10) and replaced 6 lines like this in a ~10,000 line codebase:

```python
# Before:
for first, second in zip(my_sequence, my_sequence[1:], strict=False):
    ...

# After:
for first, second in pairwise(my_sequence):
    ...
```

I have no idea how easy this would be. But if it is easy it might make a nice rule!

And in addition to being less verbose, `pairwise` is also faster on my machine.

```python
import timeit
from itertools import pairwise

short = [0] * 10
medium = [0] * 1_000
long = [0] * 1_000_000

print(timeit.timeit("list(pairwise(short))", globals=globals()))
print(timeit.timeit("list(zip(short, short[1:], strict=False))", globals=globals()))
# 0.5838822979985707
# 1.1156816439997783

print(timeit.timeit("list(pairwise(medium))", globals=globals(), number=10_000))
print(timeit.timeit("list(zip(medium, medium[1:], strict=False))", globals=globals(), number=10_000))
# 0.36417481299940846
# 0.4547197800002323

print(timeit.timeit("list(pairwise(long))", globals=globals(), number=10))
print(timeit.timeit("list(zip(long, long[1:], strict=False))", globals=globals(), number=10))
# 1.0911181840001518
# 1.499006936999649
```

Another common idiom for this uses indexes. But my gut tells me linting this would be much more difficult and error prone.

```python
for i in range(len(my_sequence) - 2):
    first, second = my_sequence[i], my_sequence[i + 1]
    ...
```

[0]: https://docs.python.org/3/library/itertools.html#itertools.pairwise

---

_Label `rule` added by @charliermarsh on 2023-03-02 04:29_

---

_Comment by @charliermarsh on 2023-03-02 04:30_

That's neat, would make for a good rule.

---

_Comment by @evanrittenhouse on 2023-03-04 21:48_

@charliermarsh i can take this if you don't think it's too difficult for a newbie - new to Rust, so may take a bit though!

---

_Comment by @charliermarsh on 2023-03-05 16:20_

@evanrittenhouse - Go for it! We can make it a rule under the `RUF` category.

---

_Comment by @evanrittenhouse on 2023-03-12 17:05_

@charliermarsh I've been working on this but am having a bit of trouble. It seems like there are a few `ExprKinds` that would come up (`Name`, `Constant`, come to mind). I think you'd want to ensure that all `Name.ids` are unique in a call to `zip()`, or that all `Constant.values` are subsets of the longest `value`. I was thinking about writing a `match` for it, but I think I'd have to cover each `ExprKind` which doesn't seem very ergonomic.

 I feel like there's a helper function to find this information - is that the case? Do you have any advice for how to get those values from the different `ExprKinds`? Are there any rules that I can check out for an example? I haven't found either the function or any applicable rules after looking for a few days, so reaching out for help if that's alright.

---

_Comment by @charliermarsh on 2023-03-13 23:12_

@evanrittenhouse - I think we probably want to check that the first arg is `ExprKind::Name`, and the second arg is `ExprKind::Subscript` with a `value` of `ExprKind::Name`, and the same name, etc.

You might find it useful to take the snippet you're trying to match against, put it in a file, and run `cargo dev print-ast foo.py` to see the AST. We can probably keep this check scoped to a pretty narrow case as defined above.

So e.g.:

```
let ExprKind::Name { id: first_id, .. } = &first_arg.node else {
  return;
}

let ExprKind::Subscript { value, .. } = &second_arg.node else {
  return;
}

let ExprKind::Name { id: second_id, .. } = &value.node else {
  return;
}

...
```


---

_Closed by @charliermarsh on 2023-03-17 03:50_

---
