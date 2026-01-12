```yaml
number: 7511
title: Move uvx shell completion to uvx --generate-shell-completion
type: pull_request
state: merged
author: bluss
labels: []
assignees: []
merged: true
base: main
head: uvx-complete
created_at: 2024-09-18T17:04:46Z
updated_at: 2024-09-20T06:57:03Z
url: https://github.com/astral-sh/uv/pull/7511
synced_at: 2026-01-12T16:07:52Z
```

# Move uvx shell completion to uvx --generate-shell-completion

---

_@bluss_

## Summary

Because a problem was found with Powershell and combining the generated completion scripts for uv and uvx, let's try separating uv and uvx command completion scripts.

The generated powershell script template can be seen in clap_complete source, and it starts with `using` directives, which makes it impossible (apparently) to concatenate two of those script outputs.

As a side effect, this is available under `uv tool run --generate-shell-completion` too.

Fixes #7482

## Test Plan

- `eval "$(cargo run --bin uvx -- --generate-shell-completion bash)"`
- Test Powershell


---

_Comment by @bluss on 2024-09-18 17:21_

Link to the powershell generator template - https://docs.rs/clap_complete/4.5.28/src/clap_complete/aot/shells/powershell.rs.html#28 - IMO we should not try to patch the output of the generator, but it seems to be possible, feel free to go that route if you want to.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-19 02:28_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-09-19 02:28_

---

_Comment by @bluss on 2024-09-19 06:05_

Cc @ilyagr if you are interested 

---

_Comment by @charliermarsh on 2024-09-20 01:19_

Why am I hitting this? 🤔 
```
❯ cargo run --bin uvx -- --generate-shell-completion
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/uvx --generate-shell-completion`
error: unexpected argument '--generate-shell-completion' found

Usage: uvx [OPTIONS] [COMMAND]

For more information, try '--help'.
```

---

_Comment by @charliermarsh on 2024-09-20 01:20_

Oh I think I had to build `uv` too.

---

_@charliermarsh approved on 2024-09-20 01:21_

---

_Comment by @charliermarsh on 2024-09-20 01:21_

Thanks for following up.

---

_Merged by @charliermarsh on 2024-09-20 01:27_

---

_Closed by @charliermarsh on 2024-09-20 01:27_

---

_Branch deleted on 2024-09-20 06:57_

---
