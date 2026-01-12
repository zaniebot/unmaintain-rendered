```yaml
number: 7046
title: Enable selection of preview rules
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: cli/preview-nursery
head: charlie/cli-preview
created_at: 2023-09-01T15:38:48Z
updated_at: 2023-09-06T19:13:39Z
url: https://github.com/astral-sh/ruff/pull/7046
synced_at: 2026-01-12T15:55:23Z
```

# Enable selection of preview rules

---

_@charliermarsh_

One proposal for supporting workflows like:

```
ruff check --select E .  # Only includes stable E rules.
ruff check --select E --preview .  # Includes stable and preview E rules.
ruff check --select E225 .  # Includes E225, even though it's in preview.
ruff check --select E225 --preview .  # Includes E225.
ruff check --select ALL .  # Includes all stable rules.
ruff check --select ALL --preview .  # Includes all stable and preview rules.
```

The basic idea is to stop filtering out nursery rules in the macro code, and instead filter them out when we resolve the rule set. This alone wouldn't support selecting individual rules (`ruff check --select E225 .`), so to fix that, we add a new specificity and `RuleCodePrefix`, for selectors that extract a single rule.

There are a few downsides to this approach:

- All selectors will now appear in the JSON Schema and be supported on the CLI, including those that grab single rules (like `E225`) and those that only include preview rules (like `E22` -- all of the rules under that prefix are in preview, but `--select E22` without the preview flag won't work). This is not ideal, but... it's also somewhat rare and should be rarer over time.
- Maybe others? Thinking...


---

_@charliermarsh reviewed on 2023-09-01 15:39_

---

_Review comment by @charliermarsh on `crates/ruff/src/rule_selector.rs`:72 on 2023-09-01 15:39_

This isn't sufficiently robust, it can be refined... (E.g., this would match `--select CPY` since there's only one rule in that category).

---

_@charliermarsh reviewed on 2023-09-01 15:40_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/configuration.rs`:489 on 2023-09-01 15:40_

Actually, here's another idea...

Instead of matching on specific selector (like `--select E225`), what if we allow the selector if _all rules_ in that selector are preview? So e.g. `--select CPY` would work, as would `--select CPY001`.

Similarly, `--select E2` _would_ turn on the `E2` rules even though they're in preview. But `--select E` would not. The idea being that you'd have to explicitly select a preview category here.

---

_@charliermarsh reviewed on 2023-09-01 15:48_

---

_Review comment by @charliermarsh on `crates/ruff/src/rule_selector.rs`:72 on 2023-09-01 15:48_

(Fixed.)

---

_Review requested from @zanieb by @charliermarsh on 2023-09-01 15:48_

---

_Comment by @codspeed-hq[bot] on 2023-09-01 15:57_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/cli-preview)

### Merging #7046 will **degrade performances by 7.11%**

<sub>:warning: No base runs were found</sub>

<sub>Falling back to comparing <code>charlie/cli-preview</code> (b9f0ade) with <code>main</code> (fbc9b5a)</sub>



### Summary

`❌ 8` regressions
`✅ 12` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/cli-preview)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/cli-preview` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[numpy/ctypeslib.py]` | 18.7 ms | 19.4 ms | -3.27% |
| ❌ | `linter/default-rules[numpy/globals.py]` | 2.2 ms | 2.3 ms | -4.38% |
| ❌ | `linter/default-rules[pydantic/types.py]` | 39.4 ms | 41.6 ms | -5.37% |
| ❌ | `linter/default-rules[large/dataset.py]` | 93.6 ms | 100.7 ms | -7.11% |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 33.9 ms | 35.3 ms | -4.1% |
| ❌ | `linter/all-rules[unicode/pypinyin.py]` | 16.9 ms | 17.3 ms | -2.33% |
| ❌ | `linter/default-rules[unicode/pypinyin.py]` | 6.4 ms | 6.7 ms | -4.92% |
| ❌ | `linter/all-rules[pydantic/types.py]` | 72.3 ms | 74.7 ms | -3.23% |


---

_@zanieb reviewed on 2023-09-01 18:29_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:489 on 2023-09-01 18:29_

I think that @MichaReiser had a really good point about preview mode possibly changing semantic model behavior and that allowing opt-in to specific preview rules without the `--preview` flag should probably not be allowed. I think if we really want opt-in to specific preview rules, we can provide `--ignore PREVIEW` to avoid turning them all on and `--select RULE` can be used to opt-in one at a time, but the `--preview` flag would still be required and the behavior of other things could change as a consequence.

---

_@MichaReiser reviewed on 2023-09-01 18:35_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/configuration.rs`:489 on 2023-09-01 18:35_

I'm a bit undecided and realized that part of the struggle comes from that it isn't entirely clear what use cases we want to enable with preview mode. Understanding that would be important in my view because a) what I pointed out isn't a problem, b) requiring `--preview` is okay, or c) a nursery category is the better solution.

---

_@zanieb reviewed on 2023-09-01 18:45_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:489 on 2023-09-01 18:45_

I think it is safer to require `--preview` for any preview behavior. Once we start allowing you to opt into it without that setting, we create a lot of complexity. We can always _expand_ the behavior later if it seems like it'd be okay, but as a starting point I'd rather be restrictive.

---

_@charliermarsh reviewed on 2023-09-01 18:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/rule_selector.rs`:198 on 2023-09-01 18:49_

@zanieb - Here's an alternative API: we remove the `into_iter()` so that there's no more implicit selection, and now require that the preview flag is passed in to the iteration API.

---

_@charliermarsh reviewed on 2023-09-01 18:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/rule_selector.rs`:200 on 2023-09-01 18:49_

(If we want to avoid enabling preview selection for specific rules, then we'd want to remove `RuleSelector::Rule { .. })` here.)

---

_@charliermarsh reviewed on 2023-09-01 18:50_

---

_Review comment by @charliermarsh on `crates/flake8_to_ruff/src/plugin.rs`:335 on 2023-09-01 18:50_

Fine to use "disabled" here, since we don't support preview in `flake8-to-ruff`.

---

_@charliermarsh reviewed on 2023-09-01 18:56_

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/defaults.rs`:76 on 2023-09-01 18:56_

I think this is maybe only used in tests...?

---

_@charliermarsh reviewed on 2023-09-01 18:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/types.rs`:197 on 2023-09-01 18:57_

This is okay (not to use preview) because this is an ignore, so it's okay if it includes rules that will never be enabled. But we should comment as such.

---

_Closed by @zanieb on 2023-09-06 19:13_

---

_Comment by @zanieb on 2023-09-06 19:13_

Work continued in https://github.com/astral-sh/ruff/pull/7195

---
