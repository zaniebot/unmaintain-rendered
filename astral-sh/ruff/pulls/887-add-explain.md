```yaml
number: 887
title: "Add `--explain`"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: explain
created_at: 2022-11-23T04:35:31Z
updated_at: 2022-11-23T05:09:17Z
url: https://github.com/astral-sh/ruff/pull/887
synced_at: 2026-01-12T15:55:05Z
```

# Add `--explain`

---

_@harupy_

This PR adds `--explain` for explaining a ruff fule.

Related to https://github.com/charliermarsh/vscode-ruff/pull/26.

```
> cargo run -- --explain C600
error: Invalid value 'C600' for '--explain <EXPLAIN>': Matching variant not found

> cargo run -- --explain C403 .
C403 (flake8-comprehensions): Unnecessary `list` comprehension (rewrite as a `set` comprehension)

> cargo run -- --explain C403 --format json .
{
  "code": "C403",
  "category": "flake8-comprehensions",
  "summary": "Unnecessary `list` comprehension (rewrite as a `set` comprehension)"
}
```

---

_@charliermarsh reviewed on 2022-11-23 04:38_

---

_Review comment by @charliermarsh on `src/cli.rs`:116 on 2022-11-23 04:38_

Can this be `Option<CheckCode>`?

---

_@harupy reviewed on 2022-11-23 04:42_

---

_Review comment by @harupy on `src/cli.rs`:116 on 2022-11-23 04:42_

Yes, changed!

---

_Comment by @charliermarsh on 2022-11-23 04:48_

Cool, this looks good to me. I'd like to spruce up `--explain` but it doesn't need to block this PR.

---

_Review comment by @charliermarsh on `src/main.rs`:314 on 2022-11-23 04:49_

I think this can be simplified to something like:

```rust
if let Some(code) = cli.explain {
    explain(&code, cli.format)?;
    return Ok(ExitCode::SUCCESS);
}
```

(The failure code should be handled automatically if `explain` returns `Err`.)

---

_@charliermarsh reviewed on 2022-11-23 04:49_

---

_Review comment by @harupy on `src/main.rs`:314 on 2022-11-23 04:50_

Great suggestion! Fixed :)

---

_@harupy reviewed on 2022-11-23 04:51_

---

_Marked ready for review by @harupy on 2022-11-23 04:53_

---

_Review comment by @harupy on `src/main.rs`:83 on 2022-11-23 04:59_

We might not need JSON. For https://github.com/charliermarsh/vscode-ruff/pull/26, just showing the text explanation is enough for now.

---

_@harupy reviewed on 2022-11-23 04:59_

---

_Merged by @charliermarsh on 2022-11-23 05:06_

---

_Closed by @charliermarsh on 2022-11-23 05:06_

---

_Branch deleted on 2022-11-23 05:09_

---
