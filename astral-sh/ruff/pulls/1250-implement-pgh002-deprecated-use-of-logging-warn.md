```yaml
number: 1250
title: Implement PGH002 - deprecated use of logging.warn
type: pull_request
state: closed
author: squiddy
labels: []
assignees: []
base: main
head: pygrep-hooks-deprecated-log-warn
created_at: 2022-12-15T19:38:25Z
updated_at: 2022-12-27T05:43:04Z
url: https://github.com/astral-sh/ruff/pull/1250
synced_at: 2026-01-12T05:36:31Z
```

# Implement PGH002 - deprecated use of logging.warn

---

_Pull request opened by @squiddy on 2022-12-15 19:38_

Refs #980

---

_Marked ready for review by @squiddy on 2022-12-15 19:38_

---

_@charliermarsh reviewed on 2022-12-15 20:36_

---

_Review comment by @charliermarsh on `src/pygrep_hooks/plugins.rs`:10 on 2022-12-15 20:36_

Is it possible to write these instead using the `call_path` utilities? Like `let call_path = dealias_call_path(collect_call_paths(expr), import_aliases);` and such that we have elsewhere? These tend to do a better job of tracking aliased imports. (The downside is they might raise false-positives when you redefine `warn`, but that feels less important to me...)


---

_@charliermarsh reviewed on 2022-12-15 20:37_

---

_Review comment by @charliermarsh on `src/pygrep_hooks/plugins.rs`:53 on 2022-12-15 20:37_

I think I'd find it easier to reason about the logic if we inverted all the cases -- so, instead of early-return, we moved this `add_check` into the few paths that _did_ trigger the error (and made the cases, like, `if id == "warn"`, and so on).

---

_Comment by @squiddy on 2022-12-16 07:54_

Thanks for the review. If you don't mind I'll followup on it either today or tomorrow.

---

_@squiddy reviewed on 2022-12-16 07:55_

---

_Review comment by @squiddy on `src/pygrep_hooks/plugins.rs`:53 on 2022-12-16 07:55_

I was wondering about that in the beginning as well. Then I just made it work first but didn't reevaluate. Happy to change that.

---

_@squiddy reviewed on 2022-12-16 08:33_

---

_Review comment by @squiddy on `src/pygrep_hooks/plugins.rs`:10 on 2022-12-16 08:33_

I'm having problems understanding this one or rather, seeing how this helps readability.

I would still have to handle at least two cases to lookup how something was imported.

```rust
let call_path = dealias_call_path(collect_call_paths(func), &checker.import_aliases);
    match &call_path[..] {
        [name] => ..,
        [module, name] => ..,
        _ => (),
    }
```

Otherwise something like `from warnings import warn; warn("hello")` would trigger the check.


---

_@charliermarsh reviewed on 2022-12-16 16:36_

---

_Review comment by @charliermarsh on `src/pygrep_hooks/plugins.rs`:10 on 2022-12-16 16:36_

Yeah, you _will_ need a dedicated check for `log.warn` since that's not based on imports at all.

But I think the check can be this? Unless I'm misunderstanding some of the intent:

```rs
let call_path = dealias_call_path(collect_call_paths(expr), import_aliases);

if match_call_path(&call_path, "logging", "warn") {
  // Not ok
} else if call_path == &["log", "warn"] {
  // Not ok
}
```

This would avoid ever flagging on any form of `warnings.warn`.

---

_@squiddy reviewed on 2022-12-17 09:10_

---

_Review comment by @squiddy on `src/pygrep_hooks/plugins.rs`:10 on 2022-12-17 09:10_

Thank you, I didn't know about `match_call_path`. My confusion stem from the fact that just `dealias_call_path` would give me `["warn"]`in some cases, which is not enough information.

---

_Closed by @squiddy on 2022-12-18 06:33_

---

_Branch deleted on 2022-12-27 05:43_

---
