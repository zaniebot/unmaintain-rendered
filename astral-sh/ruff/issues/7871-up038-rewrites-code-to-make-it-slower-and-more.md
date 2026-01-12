```yaml
number: 7871
title: UP038 rewrites code to make it slower and more verbose
type: issue
state: closed
author: AlexWaygood
labels: []
assignees: []
created_at: 2023-10-09T14:26:54Z
updated_at: 2025-03-13T23:20:17Z
url: https://github.com/astral-sh/ruff/issues/7871
synced_at: 2026-01-12T15:54:47Z
```

# UP038 rewrites code to make it slower and more verbose

---

_@AlexWaygood_

(Apologies for the slightly negative tone in this issue -- I'm a big fan of ruff, and think it's a great tool!)

---

If I have a file `foo.py`, like so:

```py
isinstance('foo', (int, bytes, dict, set, list, memoryview, str))
```

Running `ruff foo.py --select=UP --fix --target-version="py310"` on this file will make the following change:

```diff
-isinstance('foo', (int, bytes, dict, set, list, memoryview, str))
+isinstance('foo', int | bytes | dict | set | list | memoryview | str)
```

The new code is more verbose. I wouldn't mind that _so_ much, however... if it wasn't also much slower!

```
>python -m timeit "isinstance('foo', (int, bytes, list, set, dict, str))"
Running PGUpdate|x64 interpreter...
2000000 loops, best of 5: 160 nsec per loop

>python -m timeit "isinstance('foo', int|bytes|list|set|dict|str)"
Running PGUpdate|x64 interpreter...
500000 loops, best of 5: 500 nsec per loop
```

(These timings are using a ~reasonably fresh PGO-optimised non-debug build of the CPython `main` branch on Windows.)

The rule can of course be switched off in a config file for `ruff`. But I'm not sure what _advantages_ this rewrite actually has. The issue that proposed this rule (#2923) doesn't really give any motivations for wanting these `isinstance()` checks to be rewritten other than "it would be great". I also feel like the comment in https://github.com/astral-sh/ruff/issues/2923#issuecomment-1431683063 is overly dismissive of the performance concerns when the writer says that "`isinstance()` checks... are very unlikely to dominate any real-world workloads". It can absolutely cause real-world issues if an `isinstance()` check is unexpectedly slow: see https://github.com/python/cpython/issues/74690#issuecomment-1093749702 as an example.

My preferred outcome would honestly be if ruff removed this rule. I feel like it's encouraging an anti-pattern, and it means that I can't have `select = ["UP"]` in my ruff config without also adding `ignore = ["UP038"]`. If it _has_ to stay, though, I feel like the performance considerations should be loudly called out in the docs -- there's currently no mention of the potential issues here.

Note that this rule doesn't exist in pyupgrade itself, only in ruff: it was specifically rejected for pyupgrade, in part because of the performance issues: https://github.com/asottile/pyupgrade/pull/736.

---

_Label `needs-decision` added by @konstin on 2023-10-09 14:40_

---

_Comment by @konstin on 2023-10-09 14:43_

Isn't that more like a cpython bug? The pipe version is more consistent with type annotations users will be already familiar with, e.g. for `x` in `f(x: int | str)` holds true that `isinstance(x, int | str)`, but from my perspective there's no reason why a `UnionType` instance should be slower than a `tuple[type]`. (Even though we have to design the rules according to what cpython currently does)

---

_Comment by @AlexWaygood on 2023-10-09 15:06_

(Possibly relevant context: I'm a CPython core dev and a `typing`-module maintainer, though I basically stick to the pure-Python parts of the CPython repo :)

> Isn't that more like a cpython bug?

Maybe! But we already had a go at optimising `isinstance()` checks against `types.UnionType` instances: https://github.com/python/cpython/issues/91603. If you have any other ideas for how we could speed them up, feel free to file an issue ;)

I also think ruff's doing a disservice to its users if its rules are based on an imagined version of CPython that has no bugs. Whether or not it's a CPython bug is relevant to whether an issue should be filed with CPython to improve the performance here. But the CPython performance improvement, if it's doable, won't arrive until Python 3.13. Should ruff really be recommending the slower idiom to its users in the meantime, on the basis of "Well, this is CPython's problem, this code should really be faster"?

> The pipe version is more consistent with type annotations users will be already familiar with, e.g. for `x` in `f(x: int | str)` holds true that `isinstance(x, int | str)`

Absolutely, and I believe this is why PEP 604 specified that `isinstance()` and `issubclass()` should accept instances of `types.UnionType`: so that you could cleanly write code like this, which I have no quarrel with:

```py
CustomTypeAlias = str | int

def foo(x: CustomTypeAlias | bytes) -> None:
    if isinstance(x, CustomTypeAlias):
        print('foo')
    else:
        print('bar')
```

Just because you _can_ do it to make your code more DRY in cases like this, though, doesn't mean that you should change _existing_ code to make it slower just so that it makes use of an idiom that was added in a more recent Python release.

---

_Comment by @zanieb on 2023-10-09 15:12_

I think the best path forward here may be to remove this rule from the default set when we re-categorize the rules.

> makes use of an idiom that was added in a more recent Python release.

Is kind of the point of the upgrade rules, but I agree it should probably be opt-in since it has a performance cost. A comment about this in the documentation for the rule would be good.

---

_Comment by @AlexWaygood on 2023-10-09 15:18_

> > makes use of an idiom that was added in a more recent Python release.
> 
> Is kind of the point of the upgrade rules

Well, the original pyupgrade tool definitely upgrades code to use more modern syntax, and to use features introduced in more recent Python versions. But I feel like that's a means to an end than the actual _goal_, per se. This is just my subjective impression, but I always considered the goal of pyupgrade to be to transform code to make it cleaner and more elegant -- and _the way it does that_ is by upgrading the code to make use of more modern idioms. In my opinion, I don't think this rule really encourages code that's significantly cleaner or more elegant.

---

_Comment by @AlexWaygood on 2023-10-09 15:21_

Anyway, thanks for the responses! I can write up a docs PR :)

---

_Comment by @charliermarsh on 2023-10-09 18:36_

Super interesting, thanks for sharing @AlexWaygood.

I _suspect_ that if I knew this at the time, I _probably_ wouldn't have added UP038 -- though it's hard to say. While the code change has merit (with respect to improved compatibility with annotation syntax elsewhere), it has to be weighed holistically, and (CPython) performance is part of that equation.

Removing it entirely feels off at this point, but I agree with trying to categorize it out of any default ruleset once we recategorize.


---

_Comment by @Avasam on 2024-03-14 21:06_

In the mean time, can rules with fixes that create a known negative performance impact be marked as unsafe?
I can think of this one and https://docs.astral.sh/ruff/rules/suppressible-exception/

---

_Comment by @zanieb on 2024-03-15 01:26_

I don't think performance fits into our concept of fix safety. I think not raising the violation makes a lot more sense than gating the fix for the violation. 

Note you can [manually downgrade fixes to unsafe](https://docs.astral.sh/ruff/settings/#lint_extend-unsafe-fixes) in your configuration.

---

_Comment by @Avasam on 2025-02-08 20:11_

> I don't think performance fits into our concept of fix safety. I think not raising the violation makes a lot more sense than gating the fix for the violation.

Coming back to this. Could it be marked as `preview` so it's not selected by default when selecting the `UP` group ? And not selected by default even in preview mode with [explicit-preview-rules](https://docs.astral.sh/ruff/settings/#lint_explicit-preview-rules) ?

I doubt most users who have this rule enabled know that it writes code that is objectively worse (but might subjectively look better to some, in which case one can explicitly enable it)

---

Alternatively, I'm not against having a conflicting, opposite rule (in `RUF`?, `UP`?, `PERF`?) that disallows the union syntax in `isinstance` checks. So that anyone who enables both `RUF` (if a separate group) and `UP` will be warned and they can make their own choice. Also increasing visibility on this. Similar to some `DOC` rules.

The same idea is applicable [suppressible-exception (SIM105)](https://docs.astral.sh/ruff/rules/suppressible-exception/#suppressible-exception-sim105).

---

_Comment by @MichaReiser on 2025-02-08 22:26_

@AlexWaygood do you still feel the same about this rule? You now have the power to suggest the rule for deprecation 😉

---

_Comment by @AlexWaygood on 2025-02-08 23:22_

> [@AlexWaygood](https://github.com/AlexWaygood) do you still feel the same about this rule? You now have the power to suggest the rule for deprecation 😉

I do indeed haha. My opinion is still that the changes recommended by this rule only make your code worse, and have no real benefits. I still switch it off on all my Python projects.

I feel like it was a mistake in the first place for the PEP-604 implementation to make it possible to use `UnionType` instances in `isinstance()` and `issubclass()` calls: it blurs the (already fuzzy, in Python) distinction between types and runtime values. Subsequent changes to the typing system have deliberately gone in the other direction in order to clarify this distinction: for example, it's not possible to use PEP-695 type aliases in `isinstance()` or `issubclass()` calls, and that's a feature rather than a bug.

I'd love to deprecate this rule if we have consensus around that change.

---

_Added to milestone `v0.10` by @MichaReiser on 2025-02-08 23:32_

---

_Comment by @notatallshaw on 2025-02-09 00:53_

I've already turned it off on all my Python projects, and with this recent thread activity I was about to make a PR to turn it off for pip, but given pip won't be Python 3.10+ for at least a year hopefully this rule is long gone by then.

IMO the winning arguement is that type hints should not be confused with real time type checks. And the Python community has not moved towards making them look or work the same (e.g. there's been no widespread adoption frameworks that enforce type hints at run time). The performance argument is interesting, and it certainly seems like `str | int` could never be as fast as `(str, int)`, but that should perhaps be it's own performance rule(?) for those who consider the nanoseconds important.

---

_Comment by @MichaReiser on 2025-03-12 15:43_

The plan is to deprecate this rule in 0.10 and remove it in 0.11 or 0.12

See https://github.com/astral-sh/ruff/pull/16681

---

_Removed from milestone `v0.10` by @MichaReiser on 2025-03-12 15:43_

---

_Added to milestone `v0.11` by @MichaReiser on 2025-03-12 15:43_

---

_Closed by @MichaReiser on 2025-03-13 14:37_

---

_Label `needs-decision` removed by @AlexWaygood on 2025-03-13 23:20_

---
