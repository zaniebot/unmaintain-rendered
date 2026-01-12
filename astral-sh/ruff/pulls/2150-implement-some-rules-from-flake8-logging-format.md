```yaml
number: 2150
title: "Implement some rules from `flake8-logging-format`"
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: feat/implement-flake8-logging-format
created_at: 2023-01-25T08:33:56Z
updated_at: 2023-01-26T18:02:44Z
url: https://github.com/astral-sh/ruff/pull/2150
synced_at: 2026-01-12T04:52:00Z
```

# Implement some rules from `flake8-logging-format`

---

_Pull request opened by @edgarrmondragon on 2023-01-25 08:33_

https://github.com/charliermarsh/ruff/issues/1850


---

_Comment by @edgarrmondragon on 2023-01-25 21:26_

I don't think I'll implement `G100` (`extra` whitelist) or `G200` (exception instance used in log msg) for now, so this is ready for review :)

---

_Marked ready for review by @edgarrmondragon on 2023-01-25 21:26_

---

_Review comment by @charliermarsh on `src/rules/flake8_logging_format/rules.rs`:185 on 2023-01-26 01:09_

One concern here is that `PGH002` does the same thing:

```
resources/test/fixtures/flake8_logging_format/G010.py:3:1: PGH002 `warn` is deprecated in favor of `warning`
```

We could consider deprecating that rule, though, in favor of this more comprehensive plugin. We'd basically just redirect `PGH002` to `G010`.

---

_@charliermarsh reviewed on 2023-01-26 01:09_

---

_Review comment by @charliermarsh on `src/rules/flake8_logging_format/rules.rs`:157 on 2023-01-26 01:10_

In `src/rules/tryceratops/rules/error_instead_of_exception.rs`, I used some heuristics to detect whether the `value` is that of a logger (by matching on names, like `logging`, `logger`, `self.logger`, `foo_logger`, etc.). I'd like to be consistent between that rule and here. Any opinion on what's preferable?

---

_@charliermarsh reviewed on 2023-01-26 01:10_

---

_Review comment by @charliermarsh on `src/rules/flake8_logging_format/rules.rs`:137 on 2023-01-26 01:11_

Let's use `checker.resolve_call_path(expr).map_or(false, |call_path| call_path.as_slice() == ["", "dict"])` here instead. That way, it'll be robust to overrides on `dict` (not common, but good to be consistent).

---

_@charliermarsh reviewed on 2023-01-26 01:11_

---

_Review comment by @edgarrmondragon on `src/rules/flake8_logging_format/rules.rs`:137 on 2023-01-26 03:49_

Makes sense, thanks for the suggestion. Done: ad0e93bf5a2a075bedef3a8377a1378ff65a4ad2

---

_@edgarrmondragon reviewed on 2023-01-26 03:49_

---

_Review comment by @edgarrmondragon on `src/rules/flake8_logging_format/rules.rs`:157 on 2023-01-26 04:03_

I followed the plugin here to just whitelist `parser` and `warnings` but it probably makes sense to make this more robust with some `is_logger_candidate` that incorporates your heuristic additionally. Happy to try out the refactor.

---

_@edgarrmondragon reviewed on 2023-01-26 04:03_

---

_@charliermarsh reviewed on 2023-01-26 04:05_

---

_Review comment by @charliermarsh on `src/rules/flake8_logging_format/rules.rs`:157 on 2023-01-26 04:05_

That would be great, thanks!

---

_@edgarrmondragon reviewed on 2023-01-26 04:07_

---

_Review comment by @edgarrmondragon on `src/rules/flake8_logging_format/rules.rs`:185 on 2023-01-26 04:07_

I agree that it makes sense to prefer the logging plugin for this rule. Is it just a matter of adding it in `src/rule_redirects.rs`?

---

_Review comment by @edgarrmondragon on `src/rules/flake8_logging_format/rules.rs`:157 on 2023-01-26 04:43_

[29b19c6](https://github.com/charliermarsh/ruff/pull/2150/commits/29b19c6c554052dd63cf23c7e4ba0bf69894b729)

---

_@edgarrmondragon reviewed on 2023-01-26 04:43_

---

_@charliermarsh reviewed on 2023-01-26 04:46_

---

_Review comment by @charliermarsh on `src/rules/flake8_logging_format/rules.rs`:185 on 2023-01-26 04:46_

Yeah, we'd need to delete all references to the rule (fixture files, etc.), then add the redirect to that map.

---

_Merged by @charliermarsh on 2023-01-26 17:58_

---

_Closed by @charliermarsh on 2023-01-26 17:58_

---

_Branch deleted on 2023-01-26 18:02_

---
