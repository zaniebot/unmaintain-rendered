```yaml
number: 7850
title: Display the target virtual environment path if non-default
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/print-env-path
created_at: 2024-10-01T16:47:31Z
updated_at: 2024-10-18T15:12:59Z
url: https://github.com/astral-sh/uv/pull/7850
synced_at: 2026-01-12T16:08:02Z
```

# Display the target virtual environment path if non-default

---

_@zanieb_

Supersedes https://github.com/astral-sh/uv/pull/4835

Closes https://github.com/astral-sh/uv/issues/2155

e.g.

```
❯ cargo run -q -- pip install --python ../../example httpx
Using Python 3.12.1 environment at /Users/zb/example
Resolved 7 packages in 561ms
Prepared 4 packages in 62ms
Installed 4 packages in 13ms
 + certifi==2024.8.30
 + h11==0.14.0
 + httpcore==1.0.5
 + httpx==0.27.2

❯ cargo run -q -- pip install httpx
Resolved 7 packages in 17ms
Installed 4 packages in 10ms
 + certifi==2024.8.30
 + h11==0.14.0
 + httpcore==1.0.5
 + httpx==0.27.2
```

---

_Label `cli` added by @zanieb on 2024-10-01 16:47_

---

_@zanieb reviewed on 2024-10-01 16:48_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:1154 on 2024-10-01 16:48_

We see this in some tests because they change the working directory.

---

_@zanieb reviewed on 2024-10-01 16:56_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/operations.rs`:571 on 2024-10-01 16:56_

These guard against cases seen in #4835 but that implementation was in `operations::install` which is used far more broadly. In contrast, here we target the `uv pip` interface only and display a message way earlier, not during install. I think it's important to display the message earlier, as it's relevant for the resolution and audit. I think these guards are okay to retain here, though they're not used now. Willing to remove them if someone feels strongly though. 

---

_Review requested from @charliermarsh by @zanieb on 2024-10-01 21:01_

---

_@charliermarsh reviewed on 2024-10-01 21:26_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/check.rs`:15 on 2024-10-01 21:26_

Can we avoid `super`? We tend not to use that elsewhere.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/operations.rs`:573 on 2024-10-01 21:27_

Missing a word here

---

_@charliermarsh reviewed on 2024-10-01 21:27_

---

_@charliermarsh reviewed on 2024-10-01 21:27_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/operations.rs`:554 on 2024-10-01 21:27_

We should really avoid using canonicalize IMO. I've tried to remove as many of these calls as I can.

---

_@charliermarsh reviewed on 2024-10-01 21:28_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/operations.rs`:554 on 2024-10-01 21:28_

What is this case? The env doesn't exist?

---

_@zanieb reviewed on 2024-10-01 21:37_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/check.rs`:15 on 2024-10-01 21:37_

Yeah sorry that's the auto-import by rust-analyzer

---

_@zanieb reviewed on 2024-10-01 21:41_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/operations.rs`:554 on 2024-10-01 21:41_

We're canonicalizing for comparison, which I think is the correct way to compare paths? We don't display the canonicalized path and we don't fail if it fails. Why should we not here?

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/operations.rs`:554 on 2024-10-01 21:42_

We're preparing for this comparison

```rust
    // Do not report a default environment path
    if let Ok(default) = PathBuf::from(".venv").canonicalize() {
        if target == default {
           ...
```

as well as the other `starts_with` comparisons, I guess.

---

_@zanieb reviewed on 2024-10-01 21:42_

---

_@charliermarsh reviewed on 2024-10-02 00:21_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/operations.rs`:554 on 2024-10-02 00:21_

Does it do the right thing on Windows? For example, if this gets resolved to a `?\\` path, does `target.starts_with(cache.root())` return true if `cache.root()` is not a `?\\` path?

---

_@charliermarsh reviewed on 2024-10-02 00:23_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/operations.rs`:554 on 2024-10-02 00:23_

(Asking genuinely, I don't know how that behaves.)

---

_@zanieb reviewed on 2024-10-02 02:54_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/operations.rs`:554 on 2024-10-02 02:54_

I'm not sure, I'd need to try it. Basically, I think for this sort of comparison to be "generally robust" we need to be sure we're comparing paths that have been normalized the same way, ideally canonicalized but we need to fall back to absolutized paths when the path does not exist. Maybe I should just write a utility for it? I could try not canonicalizing here and see what happens, but it feels likely to be wrong? If we're avoiding storing canonicalized paths anywhere I guess it has a decent chance of being right.

---

_@zanieb reviewed on 2024-10-02 03:08_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/operations.rs`:554 on 2024-10-02 03:08_

We'll see what CI says https://github.com/astral-sh/uv/pull/7850/commits/ec07503c667f69f873ff2904d7c8aa9176c65e5d

I think since this is just a log we can easily iterate if this is wrong in practice.

---

_@charliermarsh approved on 2024-10-02 09:21_

---

_@charliermarsh reviewed on 2024-10-02 09:22_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/operations.rs`:554 on 2024-10-02 09:22_

Ok understood. I'm only pushing on it because every time I use canonicalize, I end up having to fix a bug later on!

---

_Merged by @zanieb on 2024-10-02 14:38_

---

_Closed by @zanieb on 2024-10-02 14:38_

---

_Branch deleted on 2024-10-02 14:38_

---

_Comment by @AndydeCleyre on 2024-10-16 17:12_

Can this be disabled with a flag? In my case it creates a ton of noise. 

When I use my wrapper to, for example, list installed versions of the 64 Python apps I have installed, I get 64 extra lines of this.

![image](https://github.com/user-attachments/assets/dd2d6983-a86f-4053-9d04-65321ebb9249)


---

_Comment by @zanieb on 2024-10-16 17:16_

@AndydeCleyre there's a `-q` flag, does that work for you?

---

_Comment by @AndydeCleyre on 2024-10-16 17:28_

Thanks for the quick response.

No for two reasons: 

- pip list is just one of the bulk operations I do, and for others I want to see some output (which packages are upgraded, for example).
- it completely hides the info I need:

```console
$ uv pip list --format json
Using Python 3.12.2 environment at /home/andy/.local/share/venvs/d9521a6e560469d0860c4ddb9c9baaa9/venv
[{"name":"brotli","version":"1.1.0"},{"name":"certifi","version":"2024.8.30"},{"name":"charset-normalizer","version":"3.3.2"},{"name":"idna","version":"3.10"},{"name":"mutagen","version":"1.47.0"},{"name":"pycryptodomex","version":"3.21.0"},{"name":"requests","version":"2.32.3"},{"name":"urllib3","version":"2.2.3"},{"name":"websockets","version":"13.1"},{"name":"yt-dlp","version":"2024.9.27"}]

$ uv -q pip list --format json  # (no output here)
```

---

_Comment by @charliermarsh on 2024-10-16 17:31_

I think it's a bug if `-q` is silencing the `pip list` output.

---

_Comment by @charliermarsh on 2024-10-16 17:31_

But if you're wrapping uv I feel like you almost certainly do want `-q`? Who knows what we'll print in the future.

---

_Comment by @AndydeCleyre on 2024-10-16 17:32_

> Who knows what we'll print in the future.

Even for the json-formatted output of `uv pip list`? I thought it was meant to match the json-formatted output of regular `pip list`.

---

_Comment by @charliermarsh on 2024-10-16 17:35_

Ah no, that should definitely follow an API contract! I'm more referring to the kind of output we'd suppress with `-q`, which wouldn't be covered by any API contract (it's just peripheral logging).


---

_Comment by @charliermarsh on 2024-10-16 18:16_

(Like, anything that's machine readable should still be shown under `-q`.)

---

_Comment by @AndydeCleyre on 2024-10-18 15:12_

FWIW I found I can solve my issue by directing stderr to /dev/null. 👍🏼 

---
