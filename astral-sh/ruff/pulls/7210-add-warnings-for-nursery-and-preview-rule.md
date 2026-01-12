```yaml
number: 7210
title: Add warnings for nursery and preview rule selection
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zanie/rule-warning
created_at: 2023-09-06T21:08:40Z
updated_at: 2023-09-13T20:30:00Z
url: https://github.com/astral-sh/ruff/pull/7210
synced_at: 2026-01-12T02:39:09Z
```

# Add warnings for nursery and preview rule selection

---

_Pull request opened by @zanieb on 2023-09-06 21:08_

## Summary

Adds warnings for cases where:
- A selector does not include any rules because preview is disabled
- A nursery rule is selected without the preview flag

## Test plan

Add integration tests

---

_Renamed from "zanie/rule warning" to "Add warnings for nursery and preview rule selection" by @zanieb on 2023-09-06 21:09_

---

_Comment by @github-actions[bot] on 2023-09-06 21:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:634 on 2023-09-06 22:30_

The redirect warning code has a comment that says this should be in the `ruff_cli` crate — how would we go about that? I'd like to create a tracking issue.

---

_@zanieb reviewed on 2023-09-06 22:30_

---

_Review comment by @konstin on `crates/ruff_cli/tests/integration_test.rs`:350 on 2023-09-07 08:38_

What will happen to this test when E225 gets promoted to stable? (i don't have good answer for testing this)

---

_@konstin reviewed on 2023-09-07 08:41_

---

_@zanieb reviewed on 2023-09-07 15:12_

---

_Review comment by @zanieb on `crates/ruff_cli/tests/integration_test.rs`:350 on 2023-09-07 15:12_

Great question, do we choose a new code? Should we have a couple of rules in a "TEST" category that do nothing?

---

_@zanieb reviewed on 2023-09-07 18:25_

---

_Review comment by @zanieb on `crates/ruff_cli/tests/integration_test.rs`:350 on 2023-09-07 18:25_

I'm looking into this

---

_@zanieb reviewed on 2023-09-07 18:40_

---

_Review comment by @zanieb on `crates/ruff_cli/tests/integration_test.rs`:350 on 2023-09-07 18:40_

Not working... but an idea https://github.com/astral-sh/ruff/compare/zanie/rule-warning...zanie/rule-tests

---

_@MichaReiser reviewed on 2023-09-08 06:32_

---

_Review comment by @MichaReiser on `crates/ruff_cli/tests/integration_test.rs`:350 on 2023-09-08 06:32_

I'm a bit hesitant about adding test-only rules. We would need to filter them out in documentation, schema generation and prevent they can be selected in production (but then, we want them to be selectable in tests). 

We could add the category behind a rust feature and make this an integration test (integration tests can require features), but this still suffers from the above-mentioned problems, although it has some more safeguards around it. 

To me this is an inherent downside of our statically generated rule schema (instead of deriving a `Rule` array that has somewhat weaker keys which would allow you to mock out the rules).

I don't have a good solution to propose that doesn't require changing our rule structure or abstracting some of the logic by a trait that then allows overriding whether a rule is preview or not. 

---

_Marked ready for review by @zanieb on 2023-09-11 17:32_

---

_Label `preview` added by @zanieb on 2023-09-11 18:38_

---

_Review comment by @zanieb on `crates/ruff_cli/tests/integration_test.rs`:350 on 2023-09-11 23:09_

Doesn't the `#[cfg(test)]` approach address most of those concerns?

---

_@zanieb reviewed on 2023-09-11 23:09_

---

_@zanieb reviewed on 2023-09-11 23:09_

---

_Review comment by @zanieb on `crates/ruff_cli/tests/integration_test.rs`:350 on 2023-09-11 23:09_

We can also wait and see how much of a burden it is to maintain the tests as-is. I don't like it though.

---

_@konstin reviewed on 2023-09-12 07:12_

---

_Review comment by @konstin on `crates/ruff_cli/tests/integration_test.rs`:350 on 2023-09-12 07:12_

iirc `#[cfg(test)]` is only available in the same crate and not across crates (https://users.rust-lang.org/t/what-are-the-rules-for-cfg-test/54122/2), so e.g. `ruff` `cfg(test)` rules would not be available in `ruff_cli`.

---

_@zanieb reviewed on 2023-09-12 19:04_

---

_Review comment by @zanieb on `crates/ruff_cli/tests/integration_test.rs`:350 on 2023-09-12 19:04_

Resources:
- https://nrxus.github.io/faux/guide/exporting-mocks.html
- https://github.com/rust-lang/cargo/issues/8379

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:634 on 2023-09-12 19:42_

I'd vote to just remove that TODO -- I can't remember why it's there.

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:633 on 2023-09-12 19:43_

Because this doesn't use `warn_user_once_by_id`, the warnings will probably show up multiple times if we run this configuration resolution multiple times (which we might in a given invocation -- I can't remember exactly). Is it possible to use `warn_user_once_by_id`?

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:608 on 2023-09-12 19:44_

Cool so this is like -- you selected `CPY`, but preview is disabled?

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:608 on 2023-09-12 19:44_

And this is like -- you selected `CPY`, but preview is disabled?

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:603 on 2023-09-12 19:44_

So this like: you selected `CPY001`, but preview is disabled.

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:608 on 2023-09-12 19:45_

Do both warnings show if you select `CPY001`? (Should they?)

---

_@charliermarsh reviewed on 2023-09-12 19:45_

---

_@zanieb reviewed on 2023-09-12 19:47_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:633 on 2023-09-12 19:47_

Yeah but I'd have to construct an id string like "nursery-{prefix}{code}", is that okay? I was thinking of adding a `warn_user_once_by_message` utility as an alternative? 🤷‍♀️ 

---

_@zanieb reviewed on 2023-09-12 19:48_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:608 on 2023-09-12 19:48_

Yep! I think the exact code would warn too. We could make this "safer" by adding `&& selector.rules(PreviewMode::Enabled).next().is_some()` but I think it's equivalent for now?

---

_@charliermarsh reviewed on 2023-09-12 19:56_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:633 on 2023-09-12 19:56_

I think that's okay (to add an ID like that)

---

_@zanieb reviewed on 2023-09-12 19:57_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:608 on 2023-09-12 19:57_

I can't write an integration test for exact codes yet since we don't have any preview rules.

---

_@charliermarsh reviewed on 2023-09-12 19:57_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:633 on 2023-09-12 19:57_

But I think the ID has to be a static string so that may not work...

---

_@charliermarsh reviewed on 2023-09-12 19:58_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:595 on 2023-09-12 19:58_

Does `warn_user_once!("The `NURSERY` selector has been deprecated. {suggestion}")` work?

---

_@zanieb reviewed on 2023-09-12 19:59_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:608 on 2023-09-12 19:59_

Do you mean like https://github.com/astral-sh/ruff/pull/7210/files#diff-92738d4e248765a1e48e3c930a03e20ed306f9b0c23dd67ad72912133dbb10f2R289-R306? Or if you select CPY _and_ CPY001?

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:595 on 2023-09-12 19:59_

Probably yeah I didn't realize it supported that at first and then did later :)

---

_@zanieb reviewed on 2023-09-12 19:59_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:595 on 2023-09-12 20:00_

```suggestion
                    warn_user_once!("The `NURSERY` selector has been deprecated. {suggestion}");
```

---

_@zanieb reviewed on 2023-09-12 20:00_

---

_@charliermarsh reviewed on 2023-09-12 20:00_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:608 on 2023-09-12 20:00_

I meant the former

---

_@charliermarsh approved on 2023-09-12 20:01_

The warnings look reasonable to me.

One issue we may run into (which we've discussed before and I know you're aware of) is that users may want to enable a small number of preview rules, but don't want to enable (e.g.) the logical line rules, which will get enabled by default if they add `--preview`. Not sure how to resolve... Maybe being able to `--ignore PREVIEW` like you suggested. Anyway, not a block here, just coming to mind as I look at these deprecations.

---

_Comment by @codspeed-hq[bot] on 2023-09-12 20:11_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/rule-warning)

### Merging #7210 will **degrade performances by 4.8%**

<sub>Comparing <code>zanie/rule-warning</code> (0a067cc) with <code>main</code> (6566d00)</sub>



### Summary

`❌ 5` regressions
`✅ 20` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/zanie/rule-warning)._

### Benchmarks breakdown

|     | Benchmark | `main` | `zanie/rule-warning` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[large/dataset.py]` | 154.5 ms | 161.9 ms | -4.57% |
| ❌ | `linter/all-rules[numpy/globals.py]` | 4 ms | 4.1 ms | -2.6% |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 33.2 ms | 34.3 ms | -3.2% |
| ❌ | `linter/all-rules[unicode/pypinyin.py]` | 15.4 ms | 15.7 ms | -2.29% |
| ❌ | `linter/all-rules[pydantic/types.py]` | 69.1 ms | 72.6 ms | -4.8% |


---

_Comment by @zanieb on 2023-09-12 20:34_


> Not sure how to resolve... Maybe being able to --ignore PREVIEW like you suggested. Anyway, not a block here, just coming to mind as I look at these deprecations.

Yeah I'm not sure what the proper solution for is either. We can hold off on merging the deprecations until we have a path forward? I don't have strong feelings, but we should find a decent migration story this week.



---

_Merged by @zanieb on 2023-09-13 20:29_

---

_Closed by @zanieb on 2023-09-13 20:29_

---

_Branch deleted on 2023-09-13 20:30_

---
