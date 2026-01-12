```yaml
number: 9170
title: "Turn `--verify-hashes` on by default"
type: pull_request
state: merged
author: hauntsaninja
labels:
  - security
assignees: []
merged: true
base: main
head: uv-hashes
created_at: 2024-11-17T01:08:44Z
updated_at: 2024-11-18T02:40:13Z
url: https://github.com/astral-sh/uv/pull/9170
synced_at: 2026-01-12T16:08:41Z
```

# Turn `--verify-hashes` on by default

---

_@hauntsaninja_

Fixes #9164

Using clap's `default_value_t` makes the `flag` function unhappy, so just set the default when we unwrap. Tested with no flags, `--verify-hashes`, `--no-verify-hashes` and setting in uv.toml

---

_@hauntsaninja reviewed on 2024-11-17 01:48_

---

_Review comment by @hauntsaninja on `crates/uv/tests/it/build.rs`:1557 on 2024-11-17 01:48_

I don't understand what's causing these two new lines

---

_Comment by @hauntsaninja on 2024-11-17 02:06_

Note that `UV_VERIFY_HASHES=0` won't really do anything because `flag` will receive `false, false` and turn it into `None -> true`, so we'll continue to verify hashes. We need something that does the opposite of `BoolishValueParser`

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-17 16:35_

---

_Comment by @charliermarsh on 2024-11-17 16:41_

What if we change `HashCheckingMode::from_args` to take two `Option`? Then we can differentiate between `UV_VERIFY_HASHES` being `0`, `1`, or unset.

It would also be somewhat conventional for us to make `--verify-hashes` hidden, and add `UV_NO_VERIFY_HASHES`.

---

_Comment by @hauntsaninja on 2024-11-17 20:47_

I assume you meant `flag`, not `HashCheckingMode::from_args`. Right now it's too late by then, since clap will already have given you bools. The naive thing of turning `verify_hashes` into an `Option<bool>` in the CLI struct seemed to mess with clap's logic for flags and setting `default_missing_value` didn't seem to fix. I'm probably missing some dumb clap trick, feel free to take over this PR if it's obvious to you!

---

_Comment by @charliermarsh on 2024-11-17 20:52_

Sounds good, I'll see what I can do...

---

_Comment by @hauntsaninja on 2024-11-17 20:53_

Okay, the issue is that `default_missing_value` just silently doesn't work if you don't also specify `require_equals` and `num_args`. I'll push the change (if you see a better way feel free to)!

---

_Label `security` added by @charliermarsh on 2024-11-17 20:55_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-11-18 01:18_

---

_Comment by @charliermarsh on 2024-11-18 01:37_

Okay I modified things a bit such that instead of `UV_VERIFY_HASHES=0`, we require `UV_NO_VERIFY_HASHES=1`, which is consistent with the rest of the CLI. We also now document `--no-verify-hashes` instead of `--verify-hashes`.

---

_@charliermarsh approved on 2024-11-18 01:46_

---

_Merged by @charliermarsh on 2024-11-18 01:57_

---

_Closed by @charliermarsh on 2024-11-18 01:57_

---

_Branch deleted on 2024-11-18 02:31_

---

_Review comment by @hauntsaninja on `crates/uv-static/src/env_vars.rs`:172 on 2024-11-18 02:38_

Need to change the string constant here

---

_@hauntsaninja reviewed on 2024-11-18 02:38_

---

_@hauntsaninja reviewed on 2024-11-18 02:39_

---

_Review comment by @hauntsaninja on `crates/uv-static/src/env_vars.rs`:172 on 2024-11-18 02:39_

https://github.com/astral-sh/uv/pull/9186

---

_@charliermarsh reviewed on 2024-11-18 02:40_

---

_Review comment by @charliermarsh on `crates/uv-static/src/env_vars.rs`:172 on 2024-11-18 02:40_

Hah, great... Thanks!

---
