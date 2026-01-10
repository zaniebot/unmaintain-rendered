```yaml
number: 11498
title: "[flake8-async] Sleep with >24 hour interval should usually sleep forever (ASYNC116)"
type: pull_request
state: merged
author: ekohilas
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: evan-rule-async116
created_at: 2024-05-22T20:49:51Z
updated_at: 2024-05-23T20:34:12Z
url: https://github.com/astral-sh/ruff/pull/11498
synced_at: 2026-01-10T22:05:26Z
```

# [flake8-async] Sleep with >24 hour interval should usually sleep forever (ASYNC116)

---

_Pull request opened by @ekohilas on 2024-05-22 20:49_

## Summary

Addresses #8451 by implementing rule 116 to add an unsafe fix when sleep is used with a >24 hour interval to instead consider sleeping forever.

This rule is added as async instead as I my understanding was that these trio rules would be moved to async anyway.

There are a couple of TODOs, which address further extending the rule by adding support for lookups and evaluations, and also supporting `anyio`.



---

_Review comment by @ekohilas on `crates/ruff_linter/src/rules/flake8_async/rules/sleep_forever_call.rs`:12 on 2024-05-22 20:53_

We were discussing how "no gil" was renamed to "free threading" to remove negative language.

Thoughts on changing "Why is this bad" to something like "How could it be better?" across all rules?

---

_Review comment by @ekohilas on `crates/ruff_linter/src/rules/flake8_async/snapshots/ruff_linter__rules__flake8_async__tests__ASYNC116_ASYNC116.py.snap`:1 on 2024-05-22 20:54_

Has a different testing methodology been thought about?
This feels error prone to me, since it can be easy to miss and difficult to know what exactly should pass or fail.

---

_Review comment by @ekohilas on `ruff.schema.json`:1 on 2024-05-22 20:55_

I needed this schema to be explained to me, will add some extra documentation in a [new PR](https://github.com/astral-sh/ruff/pull/11517/commits/048e0bd50742e654ffc0d93e6bd9a7d7b10fa6d7) to address this for future contributors if that's okay.

---

_Review comment by @ekohilas on `crates/ruff_linter/src/rules/flake8_async/rules/sleep_forever_call.rs`:89 on 2024-05-22 20:57_

help, as a previous c user this casting doesn't feel right to me, and @AlexWaygood and @carljm had taken a look but couldn't get this to work either.
What is the preferred way of doing this kind of logic?

---

_@ekohilas reviewed on 2024-05-22 20:59_

---

_Comment by @github-actions[bot] on 2024-05-22 21:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm reviewed on 2024-05-22 21:24_

---

_Review comment by @carljm on `crates/ruff_linter/src/rules/flake8_async/rules/sleep_forever_call.rs`:89 on 2024-05-22 21:24_

I thought we had discussed a couple alternatives to avoid this lint, e.g. rounding the float value to the next highest integer and then doing an integer comparison. Did that not work?

---

_@charliermarsh reviewed on 2024-05-23 02:11_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_async/rules/sleep_forever_call.rs`:98 on 2024-05-23 02:11_

This returns a binding that should be used below in lieu of `replacement_function.to_string()`, since in theory this could end up being `trio.sleep_forever` rather than just `sleep_forever`.

---

_@charliermarsh reviewed on 2024-05-23 02:11_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_async/rules/sleep_forever_call.rs`:44 on 2024-05-23 02:11_

Nit: more conventional in Ruff would be "Replace with `trio.sleep_forever()`". These show up in VS Code and other editors as Quick Fix titles, so they should be short and imperative.

---

_@charliermarsh reviewed on 2024-05-23 02:12_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC116.py`:51 on 2024-05-23 02:12_

Mind running `ruff format` over this to ensure (e.g.) newline consistency?

---

_Label `rule` added by @charliermarsh on 2024-05-23 02:17_

---

_Label `preview` added by @charliermarsh on 2024-05-23 02:17_

---

_@charliermarsh reviewed on 2024-05-23 02:19_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_async/snapshots/ruff_linter__rules__flake8_async__tests__ASYNC116_ASYNC116.py.snap`:1 on 2024-05-23 02:19_

The snapshot tests have been incredibly useful for us! I'm really happy with them -- we have extensive test coverage and it's really easy to add coverage + review changes once you're used to the format.

Structurally, it could be nice if each snippet were its own file. It could also be nice to somehow encode the expectation in the source itself, though I don't know that a strategy like that it is consistent with snapshotting.


---

_@ekohilas reviewed on 2024-05-23 15:15_

---

_Review comment by @ekohilas on `crates/ruff_linter/src/rules/flake8_async/snapshots/ruff_linter__rules__flake8_async__tests__ASYNC116_ASYNC116.py.snap`:1 on 2024-05-23 15:15_

> The snapshot tests have been incredibly useful for us!

Oh yes I agree! They are quite nice 😊 We had them at a past job, but the indirection in combination with the human element of checking caused errors to happen every so often 😞 

> Structurally, it could be nice if each snippet were its own file.

I was thinking something like an `ASYNC116` folder with files for each test case, but I can also image that would be annoying to edit. I assume that something like this could work instead? `cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC116/ --no-cache --preview --select ASYNC116`

> It could also be nice to somehow encode the expectation in the source itself,

Would comments work for this, like in `ASYNC116.py`?

> though I don't know that a strategy like that it is consistent with snapshotting.

What are your thoughts on if there was an extra check that did something like "each file can only have one error" or "an error must appear at the same line as a comment"?



---

_@ekohilas reviewed on 2024-05-23 15:27_

---

_Review comment by @ekohilas on `crates/ruff_linter/src/rules/flake8_async/rules/sleep_forever_call.rs`:44 on 2024-05-23 15:27_

Happy to update, although I'm not sure how unsafe edits appear in editors, should unsafe edits still be imperative?

---

_@ekohilas reviewed on 2024-05-23 15:30_

---

_Review comment by @ekohilas on `crates/ruff_linter/src/rules/flake8_async/rules/sleep_forever_call.rs`:89 on 2024-05-23 15:30_

Ah yeah, if I remember correctly we had discussed that the problem with rounding to an int was that it could possibly overflow the int and that logic would need to be checked instead.
Thus neither method was ideal and thus we needed another opinion.

---

_@ekohilas reviewed on 2024-05-23 15:31_

---

_Review comment by @ekohilas on `crates/ruff_linter/src/rules/flake8_async/rules/sleep_forever_call.rs`:44 on 2024-05-23 15:31_

Discussed at sprints with @AlexWaygood that unsafe edits should still be imperative

```suggestion
        Some(format!("Replace with `trio.sleep_forever()`"))
```

---

_Review comment by @ekohilas on `crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC116.py`:51 on 2024-05-23 15:32_

done, thoughts on having a ci job to check the format of `crates/ruff_linter/resources/test/**.*.py` ?

---

_@ekohilas reviewed on 2024-05-23 15:32_

---

_Review requested from @charliermarsh by @ekohilas on 2024-05-23 16:12_

---

_@charliermarsh approved on 2024-05-23 20:20_

Thanks!

---

_@charliermarsh reviewed on 2024-05-23 20:22_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/flake8_async/ASYNC116.py`:51 on 2024-05-23 20:22_

Unfortunately in some cases we actively _don't_ want to re-format fixtures, since we're often testing unformatted syntax intentionally (or, e.g., strange comment placement). I'd like to enable format on the fixtures, but we'd need to add suppression comments to some of the existing fixtures.

---

_@charliermarsh reviewed on 2024-05-23 20:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_async/rules/sleep_forever_call.rs`:89 on 2024-05-23 20:24_

Personally think this is ok -- we know that this constant fits in `f64`.

---

_Merged by @charliermarsh on 2024-05-23 20:25_

---

_Closed by @charliermarsh on 2024-05-23 20:25_

---
