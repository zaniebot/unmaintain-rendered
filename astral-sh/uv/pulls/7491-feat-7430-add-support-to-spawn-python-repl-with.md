```yaml
number: 7491
title: "`feat(7430)` - Add support to spawn python REPL with `uvx python` or `uvx python@<version>`"
type: pull_request
state: closed
author: mikeleppane
labels: []
assignees: []
base: main
head: feat(7430)/spawn-repl-with-uvx-python
created_at: 2024-09-18T11:37:31Z
updated_at: 2025-02-17T05:06:23Z
url: https://github.com/astral-sh/uv/pull/7491
synced_at: 2026-01-10T11:10:34Z
```

# `feat(7430)` - Add support to spawn python REPL with `uvx python` or `uvx python@<version>`

---

_Pull request opened by @mikeleppane on 2024-09-18 11:37_

## Summary

This PR adds support to spawn Python REPL with the following commands:
```bash
uvx python # or
uvx python@3.12.9 # or
uvx python@3.13.rc2
```

## Test Plan

I added a few snapshot tests to the `crates/uv/tests/tool_run.rs.` 

CI passes

### Manual Tests
```bash
cargo run -- tool run python                                                                                                                                  ░▒▓ 99% 󰁹
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.42s
     Running `target/debug/uv tool run python`
Python 3.12.1 (main, Jan  8 2024, 05:57:25) [Clang 17.0.6 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
````

```bash
cargo run -- tool run python@3.9.20                                                                                                                           ░▒▓ 99% 󰁹
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.21s
     Running `target/debug/uv tool run 'python@3.9.20'`
Python 3.9.20 (main, Sep  9 2024, 22:13:21)
[Clang 18.1.8 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

```bash
cargo run -- tool run python@3.13.0rc2                                                                                                                        ░▒▓ 99% 
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.24s
     Running `target/debug/uv tool run 'python@3.13.0rc2'`
Python 3.13.0rc2 (main, Sep  9 2024, 22:13:26) [Clang 18.1.8 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```
```bash
cargo run -- tool run python@3.13.0b3                                                                                                                         ░▒▓ 99% 
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.41s
     Running `target/debug/uv tool run 'python@3.13.0b3'`
Python 3.13.0b3 (main, Sep 12 2024, 19:13:56) [GCC 13.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

## Remarks/Questions

- `uvx python@latest` does not work, and I'm unsure how to handle this. Is this a valid alternative?
- `uvx python` launches REPL from system env? Is that correct? Or should it take the default interpreter from the virtual environment?
- [uv tool run](https://docs.astral.sh/uv/reference/cli/#uv-tool-run) has many options when running `uvx python`. E.g. `--python`?
- Do you think we should document this change somewhere? If so, what would be the correct place?


Related to issue #7430


---

_Converted to draft by @mikeleppane on 2024-09-18 11:38_

---

_Renamed from "`feat(7430)` - Add support to spawn python interpreter with `uvx python` or `uvx python<version>`" to "`feat(7430)` - Add support to spawn python REPL with `uvx python` or `uvx python<version>`" by @mikeleppane on 2024-09-18 11:48_

---

_Marked ready for review by @mikeleppane on 2024-09-18 12:52_

---

_Renamed from "`feat(7430)` - Add support to spawn python REPL with `uvx python` or `uvx python<version>`" to "`feat(7430)` - Add support to spawn python REPL with `uvx python` or `uvx python@<version>`" by @mikeleppane on 2024-09-19 06:15_

---

_Comment by @inoa-jboliveira on 2024-10-06 20:06_

I think I like #7677 approach better.
But this is not bad

---

_Assigned to @zanieb by @zanieb on 2024-10-07 12:40_

---

_Comment by @mikeleppane on 2024-10-20 17:01_

Hey, should I close this PR? It seems like no one's interested. Is this feature already implemented?

---

_Comment by @zanieb on 2024-11-18 16:14_

Sorry, I'm still interested but I think we might want a different approach to the implementation and haven't had the time to explore it myself to give a concrete recommendation.

---

_Comment by @zanieb on 2025-01-29 19:39_

I took this over in https://github.com/astral-sh/uv/pull/11076 — your feedback would be welcome!

---

_Closed by @zanieb on 2025-01-29 19:39_

---
