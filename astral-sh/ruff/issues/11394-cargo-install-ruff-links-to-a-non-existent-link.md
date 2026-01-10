---
number: 11394
title: "`cargo install ruff` links to a non-existent link now"
type: issue
state: closed
author: DeflateAwning
labels:
  - documentation
assignees: []
created_at: 2024-05-13T02:03:24Z
updated_at: 2024-05-13T18:15:49Z
url: https://github.com/astral-sh/ruff/issues/11394
synced_at: 2026-01-10T01:22:51Z
---

# `cargo install ruff` links to a non-existent link now

---

_Issue opened by @DeflateAwning on 2024-05-13 02:03_

When you run `cargo install ruff`, it links to [https://github.com/charliermarsh/ruff#installation-and-usage](https://github.com/charliermarsh/ruff#installation-and-usage), with an error message saying that it can't be installed from Cargo.

That's all fine.

The link should be updated to https://github.com/charliermarsh/ruff#installation, though. 

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-13 02:08_

---

_Comment by @charliermarsh on 2024-05-13 02:08_

I can update the link.

---

_Label `documentation` added by @charliermarsh on 2024-05-13 02:08_

---

_Closed by @charliermarsh on 2024-05-13 02:14_

---

_Comment by @dhruvmanila on 2024-05-13 07:59_

@charliermarsh was this fixed? Although I don't see that link in the output of `cargo install ruff`:

```
    Updating crates.io index
  Downloaded ruff v0.0.1
  Downloaded 1 crate (1.7 KB) in 1.78s
  Installing ruff v0.0.1
    Updating crates.io index
   Compiling ruff v0.0.1
error: Ruff is not yet available on crates.io. See https://github.com/astral-sh/ruff for alternative installations instructions.
 --> /Users/dhruv/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ruff-0.0.1/main.rs:1:1
  |
1 | compile_error!("Ruff is not yet available on crates.io. See https://github.com/astral-sh/ruff for alternative installations instructions.");
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

error: could not compile `ruff` (bin "ruff") due to 1 previous error
error: failed to compile `ruff v0.0.1`, intermediate artifacts can be found at `/var/folders/0h/7pwtlzd924l7ydmq5kwvd2dc0000gn/T/cargo-installXsXN
BZ`.
To reuse those artifacts with a future compilation, set the environment variable `CARGO_TARGET_DIR` to that path.
```

---

_Comment by @charliermarsh on 2024-05-13 13:28_

Yes, I updated the link to point to astral-sh and removed the hash which could change across versions.

---

_Comment by @DeflateAwning on 2024-05-13 18:15_

Ah too bad, that hash to the installation section made it clear what the correct way to install is. Without it, I'd suggest that the error message would probably be better if it read `Cargo not supported yet. Please install with "pip install ruff".`

Regardless, doesn't really matter I suppose.

---
