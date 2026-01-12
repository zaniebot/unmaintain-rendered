```yaml
number: 17825
title: "[`pylint`] add fix safety section (`PLC2801`)"
type: pull_request
state: merged
author: yunchipang
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/fix-safety-unnecessary-dunder-call
created_at: 2025-05-04T03:12:52Z
updated_at: 2025-05-07T22:05:22Z
url: https://github.com/astral-sh/ruff/pull/17825
synced_at: 2026-01-12T15:56:06Z
```

# [`pylint`] add fix safety section (`PLC2801`)

---

_@yunchipang_

parent: #15584 
fix was introduced at: #9587 
reasoning: #9572 

---

_Label `documentation` added by @AlexWaygood on 2025-05-04 09:59_

---

_Comment by @github-actions[bot] on 2025-05-04 10:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre reviewed on 2025-05-05 18:23_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:22 on 2025-05-05 18:23_

Like the other rule I reviewed, I think this fix is also always unsafe. Otherwise I think this explanation is pretty good. I think I would change it slightly to emphasize the custom dunder methods more heavily (as in [this comment](https://github.com/astral-sh/ruff/issues/9572#issuecomment-1900031398)). I think we handle the operator precedence correctly (barring bugs in the implementation, which should be fixed, not make a rule unsafe!), so the unsafety is from the rule not resolving dunder methods, at least based on my reading. Maybe something like this:

```suggestion
/// This fix is marked as unsafe because replacing dunder method calls with operators
/// or builtins may change the code's behavior in cases where a type only implements a
/// subset of the related methods, e.g. `__radd__`  but not `__add__`.
```

I'm not totally happy with that, but hopefully it pushes us in the right direction.

---

_@dscorbett reviewed on 2025-05-05 18:53_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:22 on 2025-05-05 18:53_

Here are two more ways the fix is unsafe because it can change behavior:
```console
$ cat >plc2801.py <<'# EOF'
class C: pass
c = C()
c.__str__ = lambda: "c"
print(c.__str__().split()[0])
print(c.__gt__(1))
# EOF

$ python plc2801.py
c
NotImplemented

$ ruff --isolated check plc2801.py --select PLC2801 --preview --unsafe-fixes --fix
Found 2 errors (2 fixed, 0 remaining).

$ python plc2801.py
<__main__.C
Traceback (most recent call last):
  File "plc2801.py", line 3, in <module>
    print(c > 1)
          ^^^^^^^
TypeError: '>' not supported between instances of 'C' and 'int'


---

_@ntBre reviewed on 2025-05-05 19:38_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:22 on 2025-05-05 19:38_

Nice, thanks. I find it so weird that these print `NotImplemented` instead of raising some kind of exception. Maybe we can summarize this as something like "... may change the program's behavior for types with custom dunder method implementations."

This isn't really relevant for this PR, but I guess we could potentially mark the fix safe if we could infer that the types are built-in? Correspondingly, we should not offer a fix if we know the dunder method has been changed, as in these examples, but I think that requires a lot more type inference in the general case, a la ty (formerly red-knot).

---

_@dscorbett reviewed on 2025-05-05 20:05_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:22 on 2025-05-05 20:05_

It can also change the program’s behavior for types _without_ custom dunder method implementations. In my example above, there are no custom `__gt__` implementations, and yet the fix for `__gt__` changes behavior. The fix is unsafe whenever the original code doesn’t do exactly what the replacement does, which is vague, but there are so many subtleties it’s hard to write a succinct yet helpful summary.

It can still be unsafe with built-in types:
```pycon
>>> (1).__gt__(1.0)
NotImplemented
>>> 1 > 1.0
False
```
But yes, you’re right that there are some combinations of built-in types and dunder methods that are safe to fix.

---

_@ntBre reviewed on 2025-05-05 20:15_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:22 on 2025-05-05 20:15_

Ah of course, thanks for the corrections. I was too eager to find a short description. We may just want to mention a few of these examples.

---

_Comment by @yunchipang on 2025-05-05 22:02_

@ntBre @dscorbett thanks again for the reviews and context. I've tried my best to consolidate the cases and put together an updated explanation. Could you kindly take a look and let me know your thoughts?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:24 on 2025-05-06 18:39_

I don't think this point is correct, and if it were I would consider it a bug in the implementation, not a reason for unsafety. When I tested the autofix in the [playground](https://play.ruff.rs/575b5290-09f5-45c2-b5ce-6872ffd3230d), it correctly parenthesized the result to `-(a - b)`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:41 on 2025-05-06 18:42_

This is also not quite right, the `split` call is what produces this output, otherwise you get something like this:

```pycon
>>> str(c)
'<__main__.C object at 0x773bfaa74ec0>'
```

It's not particularly relevant to the example at hand, but we might as well be as accurate as we can.

---

_@ntBre reviewed on 2025-05-06 18:43_

Thanks! I think this looks pretty good, with the exception of the first point and a small tweak to point (3).

---

_@dscorbett reviewed on 2025-05-06 18:48_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_dunder_call.rs`:41 on 2025-05-06 18:48_

`str` was just the first thing I thought of; `bool` is a clearer example:
```pycon
>>> c.__bool__ = lambda: False
>>> c.__bool__()
False
>>> bool(c)
True
```

---

_@ntBre approved on 2025-05-07 18:33_

Thanks!

---

_Merged by @ntBre on 2025-05-07 18:34_

---

_Closed by @ntBre on 2025-05-07 18:34_

---

_Branch deleted on 2025-05-07 22:05_

---
