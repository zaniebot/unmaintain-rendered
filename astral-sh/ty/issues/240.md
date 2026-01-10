```yaml
number: 240
title: Add a benchmark involving realistic code that creates large unions
type: issue
state: closed
author: AlexWaygood
labels:
  - performance
  - testing
  - set-theoretic types
assignees: []
created_at: 2024-09-29T16:15:19Z
updated_at: 2025-08-15T12:26:11Z
url: https://github.com/astral-sh/ty/issues/240
synced_at: 2026-01-10T02:06:24Z
```

# Add a benchmark involving realistic code that creates large unions

---

_Issue opened by @AlexWaygood on 2024-09-29 16:15_

Code that creates large unions (explicitly or implicitly) is known to be hard for type checkers to analyze in a performant way. Both mypy and pyright have had lots of issues regarding performance for these, and both have implemented several optimizations to deal with them. We should add at least one benchmark (probably several) that measures how we do on this.

Common themes that come up in this area are:
- Unions involving `Literal` types (`Literal[1, 2, 3]` desugars to `Literal[1] | Literal[1] | Literal[3]` from the type checker's perspective, so "medium-sized `Literal` types" quickly end up creating huge unions)
- Enums. A similar issue to `Literal` types. If you have an enum like this:
  ```py
  class A(enum.Enum):
      X = 1
      Y = 2
      Z = 3
  ```

  Then in some cases, `x: A` can desugar to `x: Literal[A.X] | Literal[A.Y] | Literal[A.Z]`
- Pydantic. Pydantic uses some big unions, and features in performance bug reports in both the mypy and pyright issue trackers.
- Unions involving protocols
- Unions involving recursive type aliases
- Unions involving `TypedDict`s

## References

Here are some references that are worth looking at (and from which we might be able to derive benchmarks). The great thing about all of these is that they are performance issues that we know real users encountered when their type checker was checking real code. I've tried to exclude anything specific to recursive type aliases, since that feels somewhat out of scope for this issue:

### Mypy

- https://github.com/python/mypy/issues/9169
  - fixed-ish by:
    - https://github.com/python/mypy/pull/9192
    - https://github.com/python/mypy/pull/9394
- https://github.com/python/mypy/issues/12225
- (enum-related) https://github.com/python/mypy/issues/12408
  - fixed-ish by https://github.com/python/mypy/pull/12519
- https://github.com/python/mypy/issues/12526
  - linked PRs:
    - https://github.com/python/mypy/pull/12541
    - https://github.com/python/mypy/pull/12659
- (enum-related) https://github.com/python/mypy/issues/13821
  - fixed by https://github.com/python/mypy/pull/14277
- (pydantic-related) https://github.com/python/mypy/issues/14034
  - fixed by https://github.com/python/mypy/pull/15104

### Pyright

- https://github.com/microsoft/pyright/issues/7617
  - Fixed by https://github.com/microsoft/pyright/commit/aff916f0cdebd68fc6faa416a8fb428d36a06554
- !! https://github.com/microsoft/pyright/issues/7143
  - Fixed by https://github.com/microsoft/pyright/pull/7154
- (pydantic-related) https://github.com/microsoft/pyright/issues/4781
  - Fixed by https://github.com/microsoft/pyright/commit/c7ab8045bae66a19b69ce013de6f8fb642ec403c

---

_Label `performance` added by @AlexWaygood on 2024-09-29 16:15_

---

_Comment by @dangotbanned on 2024-09-29 22:17_

We recently added a `TypedDict`-heavy feature in `altair`.

Not sure if this helps as real-world examples, but sharing as `red-knot` came up during review
- https://github.com/vega/altair/pull/3536#discussion_r1761294081

`mypy` performance here has a lot of room for improvement 

---

_Comment by @hauntsaninja on 2024-10-01 03:08_

Nice! I'll add altair to [mypy_primer](https://github.com/hauntsaninja/mypy_primer). You may also be interested in https://github.com/python/mypy/issues/17231#issuecomment-2379375287 , I think it makes mypy significantly faster on your workload.

---

_Renamed from "[red-knot] Add a benchmark involving realistic code that creates large unions" to "Add a benchmark involving realistic code that creates large unions" by @MichaReiser on 2025-05-07 15:27_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-05-11 07:18_

---

_Label `testing` added by @AlexWaygood on 2025-05-11 10:49_

---

_Comment by @MichaReiser on 2025-08-15 12:22_

@AlexWaygood we added pydantic to our benchmarks. Do you think we can close this issue or do you want to see some more union specific benchmarks?

---

_Comment by @AlexWaygood on 2025-08-15 12:26_

Yeah, we now have a bunch of benchmarks running ty over codebsaes that other type checkers have (or used to have) pathological performance issues with. I think we're in a much better position now.

I'm sure there are other benchmarks we can usefully add in the future, and the list above will still probably provide a useful reference, but I don't think there's anything immediately actionable here anymore. Thank you for adding all the new benchmarks!

---

_Closed by @AlexWaygood on 2025-08-15 12:26_

---
