```yaml
number: 18998
title: "[ty] Eagerly simplify 'True' and 'False' constraints"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/eagerly-simplify-true-false
created_at: 2025-06-27T20:01:35Z
updated_at: 2025-06-30T11:15:14Z
url: https://github.com/astral-sh/ruff/pull/18998
synced_at: 2026-01-12T15:56:29Z
```

# [ty] Eagerly simplify 'True' and 'False' constraints

---

_@sharkdp_

## Summary

Simplifies literal `True` and `False` conditions to `ALWAYS_TRUE` / `ALWAYS_FALSE` during semantic index building. This allows us to eagerly evaluate more constraints, which should help with performance (looks like there is a tiny 1% improvement in instrumented benchmarks), but also allows us to eliminate definitely-unreachable branches in control-flow merging. This can lead to better type inference in some cases because it allows us to retain narrowing constraints without solving https://github.com/astral-sh/ty/issues/690 first:
```py
def _(c: int | None):
    if c is None:
        assert False
    
    reveal_type(c)  # int, previously: int | None
```

closes https://github.com/astral-sh/ty/issues/713

## Test Plan

* Regression test for https://github.com/astral-sh/ty/issues/713
* Made sure that all ecosystem diffs trace back to removed false positives


---

_Label `ty` added by @sharkdp on 2025-06-27 20:01_

---

_Comment by @github-actions[bot] on 2025-06-27 20:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- TOTAL MEMORY USAGE: ~49MB
+ TOTAL MEMORY USAGE: ~45MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~171MB
+     memo fields = ~156MB

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~106MB

apprise (https://github.com/caronc/apprise)
- warning[possibly-unbound-attribute] test/test_plugin_email.py:369:24: Attribute `parse_url` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] test/test_plugin_email.py:370:24: Attribute `parse_url` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] test/test_plugin_email.py:371:24: Attribute `parse_url` on type `Unknown | None` is possibly unbound
- Found 4326 diagnostics
+ Found 4323 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
-     memo fields = ~88MB
+     memo fields = ~97MB

boostedblob (https://github.com/hauntsaninja/boostedblob)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~97MB

pytest (https://github.com/pytest-dev/pytest)
- warning[possibly-unbound-attribute] src/_pytest/python.py:1310:37: Attribute `stash` on type `Node | None | @Todo(map_with_boundness: intersections with negative contributions)` is possibly unbound
- Found 543 diagnostics
+ Found 542 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~189MB
+     memo fields = ~207MB

freqtrade (https://github.com/freqtrade/freqtrade)
-     memo fields = ~304MB
+     memo fields = ~276MB

django-stubs (https://github.com/typeddjango/django-stubs)
- warning[possibly-unbound-attribute] mypy_django_plugin/lib/helpers.py:153:15: Attribute `names` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] mypy_django_plugin/lib/helpers.py:161:11: Attribute `names` on type `Unknown | None` is possibly unbound
- Found 462 diagnostics
+ Found 460 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- TOTAL MEMORY USAGE: ~717MB
+ TOTAL MEMORY USAGE: ~652MB

```
</details>


---

_Marked ready for review by @sharkdp on 2025-06-30 10:24_

---

_Review requested from @carljm by @sharkdp on 2025-06-30 10:24_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-06-30 10:24_

---

_Review requested from @dcreager by @sharkdp on 2025-06-30 10:24_

---

_@AlexWaygood approved on 2025-06-30 10:53_

Very nice!

Theoretically we could also simplify constraints such as `not True` `not False`, or `not not True`, etc. But it probably wouldn't be worth the added complexity -- I don't know why you'd write code like that?! I can find a few examples on grep.app, but it doesn't seem like a useful pattern...

---

_Comment by @sharkdp on 2025-06-30 11:11_

> Theoretically we could also simplify constraints such as `not True` `not False`, or `not not True`, etc. But it probably wouldn't be worth the added complexity -- I don't know why you'd write code like that?!

Agreed.

I also grepped for some common patterns of definitely-true/false conditions and the only other thing that seemed frequently used *and* feasible to unambiguously detect in semantic index building was the `if TYPE_CHECKING` and `if not TYPE_CHECKING` pattern. I seem to remember that we don't look up if it originates from `typing` anyway? So maybe that's worth doing?

Another thing that could be very valuable for performance would be to detect certain frequently used patterns that will *definitely* lead to an *ambiguous* truthiness result later. But I'm not sure what kind of patterns that might be...

---

_Merged by @sharkdp on 2025-06-30 11:11_

---

_Closed by @sharkdp on 2025-06-30 11:11_

---

_Branch deleted on 2025-06-30 11:11_

---

_Comment by @AlexWaygood on 2025-06-30 11:15_

> I seem to remember that we don't look up if it originates from `typing` anyway? So maybe that's worth doing?

Yeah, I think that could be worth doing!

---
