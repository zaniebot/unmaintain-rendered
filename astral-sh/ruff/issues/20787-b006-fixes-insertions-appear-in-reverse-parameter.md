```yaml
number: 20787
title: B006 fixes’ insertions appear in reverse parameter order
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-10-09T13:31:11Z
updated_at: 2025-10-13T20:22:37Z
url: https://github.com/astral-sh/ruff/issues/20787
synced_at: 2026-01-10T11:09:59Z
```

# B006 fixes’ insertions appear in reverse parameter order

---

_Issue opened by @dscorbett on 2025-10-09 13:31_

### Summary

When a function has multiple fixes for [`mutable-argument-default` (B006)](https://docs.astral.sh/ruff/rules/mutable-argument-default/), the fixes’ `if` blocks appear in the opposite order of the parameters. This changes the program’s behavior if the default values have side effects or depend on each other. It’s an unsafe fix and the program’s behavior changes anyway, but it seems like it should be possible to keep the blocks in the same order as the parameters, which is probably closer to the original intent. Also, even for more normal-looking functions where it makes no behavioral difference, it would be clearer to set the parameters in the same order they appear in the signature. [Example](https://play.ruff.rs/6c9aeedc-ac49-4122-8c5b-70cded4ab311):
```console
$ cat >b006.py <<'# EOF'
def f(x=[print("1")], y=[print("2")]):
    print(x, y)
f()

def f(x=(x0 := []), y=(y0 := [x0])):
    print(x, y)
try:
    f()
except UnboundLocalError as e:
    print(e)
# EOF

$ python b006.py
1
2
[None] [None]
[] [[]]

$ ruff --isolated check b006.py --select B006 --preview --unsafe-fixes --fix
Found 4 errors (4 fixed, 0 remaining).

$ cat b006.py
def f(x=None, y=None):
    if y is None:
        y = [print("2")]
    if x is None:
        x = [print("1")]
    print(x, y)
f()

def f(x=None, y=None):
    if y is None:
        y = (y0 := [x0])
    if x is None:
        x = (x0 := [])
    print(x, y)
try:
    f()
except UnboundLocalError as e:
    print(e)

$ python b006.py
2
1
[None] [None]
cannot access local variable 'x0' where it is not associated with a value
```

### Version

ruff 0.14.0 (beea8cdfe 2025-10-07)

---

_Label `bug` added by @ntBre on 2025-10-09 13:39_

---

_Label `fixes` added by @ntBre on 2025-10-09 13:39_

---

_Comment by @IDrokin117 on 2025-10-09 15:25_

I'll take this one

---

_Assigned to @IDrokin117 by @ntBre on 2025-10-09 16:30_

---

_Comment by @IDrokin117 on 2025-10-10 19:48_

I've investigated the root cause of this behavior and possible solutions, but I don't think any of them are ideal. Here's how the diagnostic generation and fix application logic currently works:

1. All function parameters are checked sequentially, and for each parameter matching the rule, if any. https://github.com/astral-sh/ruff/blob/bbd3856de8f83c31db84ef8a104d67ae104732ee/crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs#L121-L132
2. Currently, one diagnostic is created for each parameter with two fixes: replacing the default value with `None` and inserting an `if stmt` at the beginning of the function. https://github.com/astral-sh/ruff/blob/bbd3856de8f83c31db84ef8a104d67ae104732ee/crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs#L259-L260
3. Fixes are sorted and applied in order of their start position, e.g. in the order the parameters are declared.
https://github.com/astral-sh/ruff/blob/bbd3856de8f83c31db84ef8a104d67ae104732ee/crates/ruff_diagnostics/src/fix.rs#L88-L90 https://github.com/astral-sh/ruff/blob/bbd3856de8f83c31db84ef8a104d67ae104732ee/crates/ruff_linter/src/fix/mod.rs#L144
4. Importantly, a fix application is skipped on the current iteration if its range overlaps with or comes before the range of a previously applied fix. https://github.com/astral-sh/ruff/blob/bbd3856de8f83c31db84ef8a104d67ae104732ee/crates/ruff_linter/src/fix/mod.rs#L87-L89

Given the above, here's the algorithm for fixing the following code:

```python
def f(x=[print("1")], y=[print("2")]):
    pass
```

1. Two diagnostics are created for **x** and **y** with their corresponding fixes.
2. The fix for x is applied:

```python
def f(x=None, y=[print("2")]):
#       ^x_edit.start()
    if x is None:
        x = [print("1")]
#                       ^x_edit.end()
```

3. Since the fix for **y** starts within the range where the fix for **x** was applied, it's skipped.
4. The workflow returns to step 1, re-checking the rule for the function. This time, only 1 diagnostic with a fix for **y** is created.
5. Since the if statement must be inserted at the beginning of the function, the if statement for **y** will appear before the if statement for **x**.

Possible solutions:

1. Check parameters in reverse order for a single function and only apply the fix for the last parameter. This way, if statements would be added in reverse order. *Drawback:* Only the fix for 1 diagnostic would be shown in the preview. This is the simplest implementation.
2. Insert if statements after all existing `if parameter is None` statements instead of at the beginning of the function body. *Drawback:* User-defined if statements would also be considered.
3. Generate a single fix and attach it to the first diagnostic. *Drawback:* Potential issues with the first diagnostic containing fixes that don't belong to it.
4. Generate a single diagnostic with one fix per function, regardless of the number of parameters. *Drawback:* Issues with suppression and the use of secondary annotations.

@ntBre What are your thoughts on this?

---

_Comment by @ntBre on 2025-10-10 21:08_

Thank you for looking into this so carefully! Based on your findings, I'm kind of tempted not to make any changes here. 

Another option is to clone the same multi-parameter fix across each of the diagnostics. We have other rules like that, such as `F401`:

```
> ruff check --preview --select F - <<EOF
import math, sys
EOF
F401 [*] `math` imported but unused
 --> -:1:8
  |
1 | import math, sys
  |        ^^^^
  |
help: Remove unused import
  - import math, sys

F401 [*] `sys` imported but unused
 --> -:1:14
  |
1 | import math, sys
  |              ^^^
  |
help: Remove unused import
  - import math, sys

Found 2 errors.
[*] 2 fixable with the `--fix` option.
```

but I hadn't really considered how that interacted with the new fix display in preview. I think we may want to avoid adding more cases like that.

---

_Comment by @IDrokin117 on 2025-10-11 15:53_

@ntBre Makes sense, but I agree it would be better to leave this behavior untouched as is. My proposal is to add information about reverse order to "[Known problems](https://docs.astral.sh/ruff/rules/mutable-argument-default/#known-problems)". What do you think?

---

_Comment by @ntBre on 2025-10-13 20:22_

Hmm, I usually think of `Known problems` as more like the rule applying when it shouldn't. This is more of an implementation detail, but I guess if we can't fix it easily, it makes sense to point it out in the docs.

It might fit a little better in the `Fix Safety` section actually since it also changes the evaluation order.

---
