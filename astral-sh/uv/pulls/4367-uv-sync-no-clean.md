```yaml
number: 4367
title: "`uv sync --no-clean`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - preview
assignees: []
merged: true
base: main
head: uv-sync-no-remove
created_at: 2024-06-17T18:58:51Z
updated_at: 2024-06-17T20:24:23Z
url: https://github.com/astral-sh/uv/pull/4367
synced_at: 2026-01-10T13:54:02Z
```

# `uv sync --no-clean`

---

_Pull request opened by @ibraheemdev on 2024-06-17 18:58_

## Summary

Adds a `--no-clean` flag to `uv sync` that keeps extraneous installations. This is the default in `uv run` and `uv add`, but not in `uv sync` or `uv remove`. This means you need to run an explicit `uv sync/remove` to clean the virtual environment.

Could use some tests.

---

_Review requested from @zanieb by @ibraheemdev on 2024-06-17 18:59_

---

_Label `preview` added by @ibraheemdev on 2024-06-17 19:02_

---

_Comment by @zanieb on 2024-06-17 19:16_

I'm not sure I love the name. Are there some examples of the flag in other tools?

---

_Comment by @ibraheemdev on 2024-06-17 19:22_

The name is from [a Poetry proposal](https://github.com/python-poetry/poetry/issues/648#issuecomment-483064487), although it was not accepted. Most other tools [have the opposite default behavior](https://github.com/python-poetry/poetry/issues/648#issuecomment-483064487), so I don't know of much prior art here.

---

_Comment by @zanieb on 2024-06-17 19:32_

I don't think we have a clear concept of "tracked" and it doesn't have a negative which follows our typical "no" prefix. I think a user could change the default sync behavior via configuration and want to opt-in to removal via the CLI and that should be natural.

Just to throw some ideas out....

- `--no-remove-untracked` / `--remove-untracked`
- `--allow-untracked` / `--no-allow-untracked`
- `--no-remove-extra-packages` / `--remove-extra-packages`
- `--no-remove-unknown` / `--remove-unknown` 
- `--no-clean` / `--clean`
- `--no-remove` / `--remove`
- `--untracked <remove | warn | ignore>`
- `--extra-packages <remove | warn | ignore>`



---

_Comment by @zanieb on 2024-06-17 19:50_

@ibraheemdev can we separate fixing `run` and `add` so we can release without figuring out a name? Alternatively we can just use something and break it later if we want.

---

_Comment by @ibraheemdev on 2024-06-17 19:58_

`--clean` and `--no-clean` are synonymous with commands from other tools (`cargo clean`, `yarn cache clean`), so I think that makes sense for now.

---

_@zanieb approved on 2024-06-17 20:04_

---

_Comment by @zanieb on 2024-06-17 20:04_

It's a little weird because it refers to the cache in those cases but the actual environment in this one. I'm fine to try it out though.

---

_Comment by @ibraheemdev on 2024-06-17 20:05_

There's also `pdm sync --clean`.

---

_Renamed from "`uv sync --keep-untracked`" to "`uv sync --no-clean`" by @ibraheemdev on 2024-06-17 20:06_

---

_Comment by @zanieb on 2024-06-17 20:16_

That's where I saw it :D 

---

_Merged by @ibraheemdev on 2024-06-17 20:24_

---

_Closed by @ibraheemdev on 2024-06-17 20:24_

---

_Branch deleted on 2024-06-17 20:24_

---
