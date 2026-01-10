```yaml
number: 15507
title: "[`pylint`] Do not report methods with only one `EM101`-compatible `raise` (`PLR6301`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: PLR6301
created_at: 2025-01-15T16:05:25Z
updated_at: 2025-01-17T12:53:53Z
url: https://github.com/astral-sh/ruff/pull/15507
synced_at: 2026-01-10T20:34:00Z
```

# [`pylint`] Do not report methods with only one `EM101`-compatible `raise` (`PLR6301`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-15 16:05_

## Summary

Related to [this comment](https://github.com/astral-sh/ruff/issues/12172#issuecomment-2592625245) by @DaniBodor at #12172. Tests adapted from that of #13714.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-15 16:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-01-16 09:34_

Thanks for working on this change but I'm not convinced this goes into the right direction. 

Taking the exmaples from your tests. The `self` parameter is unused and unnecessary because `Foo` doesn't extend any base class. That's why I think this is in the spirit of `PLR6301` to flag the `self` usage. I can see how the warning is annoying when prototyping an API but I'd simply ignore the warning in that case (either using a `noqa` or literally ignoring the warning). 

The other case is where `Foo` extends from a base class but our recommendation there is to mark the method as `@override`. 

That's why I think we should leave the rule as is but I'm interested in your perspective.

---

_Label `rule` added by @MichaReiser on 2025-01-16 09:34_

---

_Label `needs-decision` added by @MichaReiser on 2025-01-16 09:34_

---

_Comment by @InSyncWithFoo on 2025-01-16 14:07_

I still advocate for this change, as it improves consistency. Currently this triggers `PLR6301` but not [`ARG002`](https://docs.astral.sh/ruff/rules/unused-method-argument/):

```py
class Foo:
    # PLR6301: Method `lorem` could be a function, class method, or static method
    def lorem(self, v):
        msg = 'Lorem ipsum dolor sit amet'
        raise NotImplementedError(msg)
```

This triggers both:

```py
class Foo:
    # PLR6301: Method `lorem` could be a function, class method, or static method
    def lorem(self, v):  # Unused method argument: `v`
        print(1 + 2)
```

---

_Label `needs-decision` removed by @MichaReiser on 2025-01-17 09:17_

---

_Label `preview` added by @MichaReiser on 2025-01-17 09:17_

---

_Merged by @MichaReiser on 2025-01-17 09:17_

---

_Closed by @MichaReiser on 2025-01-17 09:17_

---

_Branch deleted on 2025-01-17 12:53_

---
