---
number: 3011
title: F821 false positive for .pyi
type: issue
state: closed
author: JonathanPlasse
labels:
  - rule
assignees: []
created_at: 2023-02-18T15:43:10Z
updated_at: 2024-04-07T00:15:59Z
url: https://github.com/astral-sh/ruff/issues/3011
synced_at: 2026-01-07T13:12:14-06:00
---

# F821 false positive for .pyi

---

_Issue opened by @JonathanPlasse on 2023-02-18 15:43_

```python
class Distribution(IMetadataProvider): ...
class IMetadataProvider: ...
```

`ruff` detects the `F821` while `flake8` with `flake8-pyi` does not (`flake8-pyi` must be installed otherwise it will also detect an error).

[Link to the playground](https://play.ruff.rs/#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksufkoirhwFbGQqemZDbmw-CgYlLgaPAAWugAcQ43FpeVCbKIAojHTUABikGyVojxKGig88jXEnHi6kAAO0gDMAIwu86L4sNAA1ihDGNC46Rt8eIJgoBCiMlOGH4nB2mR4hFqunwcGKsVExEIx2OmWIxAw4iksk2PxhsDhC0giORqPR6VwXXBkNw-A00NhKHhUDgiCQRQ2ilemleuGk9PxjMJzQyXSI8FkhFwPGkx3Q-OKbAAvo07o9nhgyG80KDdH9-pB+ko0Dg0OgMDwJMcMZxMmUMnypv99QB6C3HG5OqDOhSKV2Wj1OyDO01yZ3EfoSFZOtb6rD9FBYB7mmVm+JYFDHL64eUoJUq+5PF4cDRkFBKXWE6qFThSQgbChdLAsn5MPO5VWFjVcXj8H560Qcbh8ATAtIijrESYMRht24F9U1RQSYh0uj9qASaBadnKXrdJJ9QZ0FyzqAd9UAR0ICESfaFuA6FIwV5vVCOaAQhAbAY3hB7j66F9b3fT9vyZMQECwSddw0Z9r2AugIK-SgfxgOQEE4TAqCbWVdB4RRCFzCBlXbecXnGfAK0DYVMgwOoJDfR1PUgIxNyeH9RCMaBiG1MoOKgIxMmOe5034lj8E4Eo0GoJlOIkqSjFNGE-x4agFhnYj8zVF4+DQWQa2ODJVKo-VNVwdQSmgPg5C6AyjJ+E4lCSYzwLMilMGgY5OF1RVT0gc8dJTbB40TWkTIRfDOD4ug8QJQN4hQS0dgkBBxEoFsFnWGUwvUvyAuBS0OiwUEd2wPAmyndcYHgZAMAkFTuE6PCCKIsASLnbSCsMxQdhwXAbKlA4BHC5kOm4xjfkyqBji1bijmONAxNOJRFGQeboCWxAeFgaQADp4hRKgHOOHRZKgYpyAybNEOIYazsgApiAyO41sQnhKPu3BJFOI5cHde7Nx4YSbw6MgjkKsTAeB7bODIXbTmh+beCWhB7kUY7YCWt4Snm7N7rgDYbSOAmxP6VGEDkSSkAc-o5Cjf5fM00jOtOW8dknaQULXQkJISSEuhmzIpXjYofnwwjwMF6AGMi2wunoqhk1wt6kRQyWlGl7ZlDl+Q4EI9FpWVqAJ3KdXFE12WugUWB9fUWrDayFXhKycDzc4UWLKvG0ukBuMMHwDJsimyAACFxpmLR00zIaxIjqOszEgA1PWUBmRRVsUMSAHkAGU04zsSAEks-zwP7pmfqbTwBipVLzP7qexNtn29PA-U13oHdxWq0wTIvdo33+n9wOhBjAElCTQXnJFibxda9qzzIjBJRIM1NkkZy72osdaJ9FReLozXcQZPz3aMkb-IydMMCQc2rTgFRRZzcCA8Ua-9l6NQAOfwkP40L-OgYDTDbfY3w2iBhwBIMggDuJdXsj-QMxBhIlTwOaN2j5YKQM3M1CWhIMimk5GQWQDscGCkDK-d+Rkb6gn6PwIoCYszHwFC-K+XQeAIHNAgd0o9wIPHSEgcyEl0Y7EFtKcB+o+HIHMgMG0mBREOmnLw-h5lEBNlgMPWABDxGiHyObdks1FCYFBubRQCiciBkyPcaytlLT2QwPgmoRwiA9RFjsdh2BECi1NoSPuXBMgpFsT1FsY8oBYHuGiCa5jYzfA2FKYJ4E96qEiSEyA6RuidHRKWV+jtFGEgAuiaA+BEicjskE3QGALjgXyRqbYqAkhKwmi4Fhb8zTFClokCofkJBYCbKWC+m5tyQOdloUECirh+VlMcF49Ewrc23i0eWR8g6enOtsAAqv9YOiQlAABEpFiWKDwDZABhcJMktllkUHsgRpzuLnJWUSdZxwACyqU-wuwubsqRry0ofIedxaQuAsA5yeWJAFQKAAqlzrl42DocjZULJw7Kshte6MJuD8zjhmBO91EC9GeUdaAWxIZbh2ZwfAlF26EjCXcmWZNMCmhwObdh6NlnMRpWiOlqV6YQBSZOKyUUuUMoTBkKyGQMoPP5XwLAQqeVgA0m1RopwTRUGlFzSa8yRT2JsooPFsENjECTFgzeOYJnSA-FBNVOTgALxOJzfglVCQslqpuDQUVdY2zYSmCVzFoJiSIcBKlgYBkanNkCkWugLgACZwIhqpLUH4AA2WNW4uQaB+AAVhTduKViUTV0AzSeJmohThIg0ObU0F8ngZnUJKPgDElazLALFeeIBFSRA7Ry4gYAyXQVhnWIaBhC4Eo2GgFFAAFValMCFhDoLtedIAu1gGHdsaAY6NiTopphGoc6F1AA)

---

_Comment by @ngnpope on 2023-02-18 18:10_

Surely this is correct though? At the time of declaration of `Distribution` it is necessary for `IMetadataProvider` to have been already declared.

---

_Comment by @JonathanPlasse on 2023-02-18 18:12_

No, it is allowed in stub files, this is taken from `typeshed`. Stub files are only read by linters never executed.

---

_Comment by @ngnpope on 2023-02-18 18:16_

Ok. Fair enough, I know declarations in stub files _can_ be out of order. I guess I'm wondering why the declarations cannot just be reordered?

---

_Comment by @JonathanPlasse on 2023-02-18 18:22_

You are right even if it is allowed does not mean it should be done.
Should it be fixed?

---

_Comment by @JonathanPlasse on 2023-02-18 18:23_

I vote fixing it.

---

_Comment by @charliermarsh on 2023-02-19 13:47_

Can you look into how `flake8-pyi` suppresses these? What mechanism is it using? I'm surprised.

---

_Label `question` added by @charliermarsh on 2023-02-19 13:47_

---

_Comment by @JonathanPlasse on 2023-02-19 15:24_

This is done [here](https://github.com/PyCQA/flake8-pyi/blob/main/pyi.py#L256-L277).
When you run `flake8` with `flake8-pyi` installed with `--verbose` you can see this
```console
❯ flake8 stubs/setuptools/pkg_resources/__init__.pyi --verbose
flake8.checker            MainProcess     91 INFO     Making checkers
flake8.bugbear            MainProcess    107 INFO     Optional warning B950 not present in selected warnings: None. Not firing it at all.
flake8.pyi                MainProcess    119 INFO     Replacing FlakesChecker with PyiAwareFlakesChecker while checking 'stubs/setuptools/pkg_resources/__init__.pyi'
flake8.main.application   MainProcess    194 INFO     Finished running
flake8.main.application   MainProcess    194 INFO     Reporting errors
flake8.main.application   MainProcess    195 INFO     Found a total of 209 violations and reported 0
```

---

_Comment by @charliermarsh on 2023-02-19 15:28_

Woah that's wild!

---

_Comment by @charliermarsh on 2023-02-19 15:29_

Yeah, we should probably just change the behavior of `ast.rs` on `pyi` files to match whatever they do there then.

---

_Referenced in [astral-sh/ruff#3909](../../astral-sh/ruff/issues/3909.md) on 2023-04-07 23:36_

---

_Referenced in [Toufool/AutoSplit#207](../../Toufool/AutoSplit/pulls/207.md) on 2023-04-08 00:24_

---

_Label `question` removed by @charliermarsh on 2023-05-08 22:54_

---

_Label `core` added by @charliermarsh on 2023-05-08 22:54_

---

_Referenced in [astral-sh/ruff#7136](../../astral-sh/ruff/issues/7136.md) on 2023-09-04 21:41_

---

_Comment by @GabDug on 2023-09-17 16:09_

Hey!

Are there any plans for this issue, now that flake8-pyi has been fully integrated?

Thanks and have a great day!

---

_Referenced in [python/typeshed#10909](../../python/typeshed/pulls/10909.md) on 2023-10-18 20:49_

---

_Referenced in [python/typeshed#11496](../../python/typeshed/pulls/11496.md) on 2024-02-29 04:30_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-03-07 15:07_

---

_Comment by @AlexWaygood on 2024-03-12 10:36_

In  #10341, I fixed the following F821 false positive on `.pyi` files:

```py
x: int
y = x  # F821 previously incorrectly emitted here in a `.pyi` file
```

But Ruff still emits an error on the original snippet in this issue if it's linting a `.pyi` file:

```py
class Distribution(IMetadataProvider): ...  # F821 still emitted here currently due to the forward reference
class IMetadataProvider: ...
```

However, with the latest version of flake8-monkeypatched-by-flake8-pyi, flake8 also emits an F821 error on that snippet. This is due to changes we made in flake8-pyi in https://github.com/PyCQA/flake8-pyi/pull/364. The motivation for the flake8-pyi changes were:
- We wanted to reduce the amount of monkeypatching we were doing to a core minimum.
- We concluded that, although this kind of forward reference is _legal_ in a stub file, it is never _necessary_ when it comes to base classes (and other similar situations), and should probably be considered bad style -- so forbidding it was probably a good thing anyway.

I'm not sure that ruff should necessarily make the same decisions that flake8-pyi made, however:
- We don't have the same concerns about keeping monkeypatching to a minimum, since we're not doing any monkeypatching at all
- F821 is a rule that's concerned with correctness, and a forward reference like this isn't a _correctness_ issue in a stub -- unlike in a `.py` file, it's perfectly legal; it's just bad style. So maybe we should avoid emitting F821 here, and emit a diagnostic with a different error code instead, to make it clear that we're banning it out of stylistic concerns rather than correctness concerns?

Curious for @charliermarsh's opinion here!

---

_Comment by @charliermarsh on 2024-03-12 18:54_

Thanks for the clear write-up. I think my preference would be...

1. Not emitting anything.
2. Emitting a new, dedicated rule.
3. Emitting F821 (which feels incorrect, as you point out above).


---

_Comment by @AlexWaygood on 2024-03-12 18:59_

I'd definitely prefer it if we emitted _something_ on these constructs in a stub file -- type checkers don't flag them (since they're legal in `.pyi` files), and it's honestly pretty confusing if you come across a class in a stub file that inherits from a class that hasn't been defined yet 😄

So that implies (2). I'll start off by making a PR that stops ruff from emitting F821 on this kind of construct, and then look at implementing a new rule for this.

Should the new rule go in the RUF ruleset?

---

_Comment by @charliermarsh on 2024-03-12 19:26_

Sounds good. Yeah, unless we want to add it to `PYI` and coordinate with `flake8-pyi` to reserve that code.

---

_Label `rule` added by @dhruvmanila on 2024-03-13 07:56_

---

_Label `core` removed by @dhruvmanila on 2024-03-13 07:56_

---

_Referenced in [astral-sh/ruff#10779](../../astral-sh/ruff/pulls/10779.md) on 2024-04-04 20:43_

---

_Referenced in [astral-sh/ruff#10788](../../astral-sh/ruff/pulls/10788.md) on 2024-04-05 11:11_

---

_Closed by @AlexWaygood on 2024-04-07 00:16_

---

_Referenced in [python/typeshed#11771](../../python/typeshed/pulls/11771.md) on 2024-04-16 16:33_

---

_Referenced in [astral-sh/ruff#15677](../../astral-sh/ruff/issues/15677.md) on 2025-01-22 22:27_

---
