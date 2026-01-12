```yaml
number: 597
title: Remove needless return
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: remove-needless-return
created_at: 2022-11-05T11:43:31Z
updated_at: 2022-11-05T18:11:27Z
url: https://github.com/astral-sh/ruff/pull/597
synced_at: 2026-01-12T05:48:45Z
```

# Remove needless return

---

_Pull request opened by @harupy on 2022-11-05 11:43_

_No description provided._

---

_@harupy reviewed on 2022-11-05 11:44_

---

_Review comment by @harupy on `src/cst/matchers.rs`:7 on 2022-11-05 11:44_

Not sure why CI doesn't catch this, but I got this warning on my machine:

```
> cargo clippy
    Checking ruff v0.0.101 (/home/haru/Desktop/repositories/ruff)
warning: unneeded `return` statement
 --> src/cst/matchers.rs:7:19
  |
7 |         Err(_) => return Err(anyhow::anyhow!("Failed to extract CST from source.")),
  |                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ help: remove `return`: `Err(anyhow::anyhow!("Failed to extract CST from source."))`
  |
  = note: `#[warn(clippy::needless_return)]` on by default
  = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#needless_return

warning: `ruff` (lib) generated 1 warning
    Finished dev [unoptimized + debuginfo] target(s) in 1.51s
```

```
> cargo clippy --version
clippy 0.1.65 (897e375 2022-11-02)
```

---

_Review comment by @charliermarsh on `src/cst/matchers.rs`:7 on 2022-11-05 18:11_

I think it's because CI is using nightly (`cargo +nightly clippy --all`), I see the same behavior locally when toggling between nightly and stable. I don't understand why though.

---

_@charliermarsh reviewed on 2022-11-05 18:11_

---

_Merged by @charliermarsh on 2022-11-05 18:11_

---

_Closed by @charliermarsh on 2022-11-05 18:11_

---
