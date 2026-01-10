```yaml
number: 4436
title: Fails to install on Termux 
type: issue
state: closed
author: edshamis
labels:
  - bug
  - release
assignees: []
created_at: 2023-05-15T09:49:20Z
updated_at: 2025-07-01T10:19:56Z
url: https://github.com/astral-sh/ruff/issues/4436
synced_at: 2026-01-10T11:09:47Z
```

# Fails to install on Termux 

---

_Issue opened by @edshamis on 2023-05-15 09:49_

There's a dedicated ruff package for Termux, but ruff-lsp and python-lsp-ruff fail to install for the same reason

```
❯ pip install --upgrade ruff
Collecting ruff
  Using cached ruff-0.0.267.tar.gz (1.1 MB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... error
  error: subprocess-exited-with-error

  × Preparing metadata (pyproject.toml) did not run successfully.
  │ exit code: 1
  ╰─> [22 lines of output]
      error: failed to get `ruff` as a dependency of package `ruff_cli v0.0.267 (/data/data/com.termux/files/usr/tmp/pip-install-og631z7k/ruff_b22440ecd7e94053a309acbac4637fe9/crates/ruff_cli)`

      Caused by:
        failed to load source for dependency `ruff`

      Caused by:
        Unable to update /data/data/com.termux/files/usr/tmp/pip-install-og631z7k/ruff_b22440ecd7e94053a309acbac4637fe9/local_dependencies/ruff

      Caused by:
        failed to parse manifest at `/data/data/com.termux/files/usr/tmp/pip-install-og631z7k/ruff_b22440ecd7e94053a309acbac4637fe9/local_dependencies/ruff/Cargo.toml`

      Caused by:
        error inheriting `colored` from workspace root manifest's `workspace.dependencies.colored`

      Caused by:
        failed to find a workspace root
      💥 maturin failed
        Caused by: Cargo metadata failed. Does your c
rate compile with `cargo build`?
        Caused by: `cargo metadata` exited with an error:
      Error running maturin: Command '['maturin', 'pep517', 'write-dist-info', '--metadata-directory', '/data/data/com.termux/files/usr/tmp/pip-modern-metadata-9wm3nkcx', '--interpreter', '/data/data/com.termux/files/usr/bin/python3']' returned non-zero exit status 1.
      Checking for Rust toolchain....
      Running `maturin pep517 write-dist-info --metadata-directory /data/data/com.termux/files/usr/tmp/pip-modern-metadata-9wm3nkcx --interpreter /data/data/com.termux/files/usr/bin/python3`
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
error: metadata-generation-failed

× Encountered error while generating package metadata.
╰─> See above for output.

note: This is an issue with the package mentioned above, not pip.
hint: See above for details.

```

---

_Comment by @edshamis on 2023-05-15 15:24_

Reported upstream: PyO3/maturin#1609

---

_Label `bug` added by @charliermarsh on 2023-05-15 15:31_

---

_Label `release` added by @charliermarsh on 2023-05-15 15:31_

---

_Closed by @konstin on 2023-05-18 17:02_

---

_Comment by @bmeares on 2024-06-21 06:39_

Do you know where I can find documentation on how to install `ruff` in Termux?

---

_Comment by @AndrewLauu on 2024-08-28 14:34_

> Do you know where I can find documentation on how to install `ruff` in Termux?

You may need to use `pkg install ruff` in Termux instead of pip

---

_Comment by @egeres on 2024-09-26 12:24_

`pkg install ruff` worked for me, as of version `0.6.7` [they seem to provide binaries for ARMv7](https://github.com/astral-sh/ruff/releases/tag/0.6.7)

---

_Comment by @shankar-narayan-k on 2025-07-01 10:19_

Hi @AndrewLauu 
While `pkg install ruff` works, there is one more place this fails, if you try to include ruff as a dependency in your project the same issue comes out again.

From my limited research, I understood the issue has something to do with Android's Phantom Process Killer.

Check here for more details: [https://github.com/termux/termux-app/issues/2366](https://github.com/termux/termux-app/issues/2366)

This is the solution that worked for me:

```sh
uv -vvv add ruff
```

The first install will take a while, but you should be good to go after that.

Do note this issue is not specific to ruff. Many PyPI packages that are built with maturin or have cargo crates as dependencies face the same problem.

And a similar approach using pip in venv shows a very low probability of success.

---
