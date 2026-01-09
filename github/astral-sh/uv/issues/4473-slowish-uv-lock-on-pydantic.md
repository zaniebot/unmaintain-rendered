---
number: 4473
title: "slowish `uv lock` on pydantic"
type: issue
state: closed
author: BurntSushi
labels:
  - performance
  - benchmarks
assignees: []
created_at: 2024-06-24T13:52:18Z
updated_at: 2024-06-24T21:09:38Z
url: https://github.com/astral-sh/uv/issues/4473
synced_at: 2026-01-07T13:12:17-06:00
---

# slowish `uv lock` on pydantic

---

_Issue opened by @BurntSushi on 2024-06-24 13:52_

To reproduce:

```
$ git clone https://github.com/pydantic/pydantic
$ <edit pyproject.toml to copy [tool.pdm.dev-dependencies] section to existing [project.optional-dependencies] section>
$ hyperfine -w1 --prepare 'rm uv.lock' 'UV_PREVIEW=1 uv lock'
```

Some light profiling shows a lot of time being spent in `_PvEval_EvalFrameDefault`. This seems to happen even when running `uv lock` without `rm uv.lock`, although it is about 2x faster when not removing the lock file. I think perhaps there is some kind of caching problem that is unnecessarily leading to rebuilding.

---

_Label `performance` added by @BurntSushi on 2024-06-24 13:52_

---

_Label `benchmarks` added by @BurntSushi on 2024-06-24 13:52_

---

_Assigned to @charliermarsh by @BurntSushi on 2024-06-24 13:52_

---

_Comment by @charliermarsh on 2024-06-24 17:39_

About 150ms is spent getting the project dependencies (since it uses dynamic metadata). But the main reason it's slow is because we have to do Git lookups every time. I think that's just unavoidable, unless we're content with using stale refs.

---

_Comment by @charliermarsh on 2024-06-24 19:39_

With #4482 and #4487, I now get 506ms for `rm uv.lock && uv lock` and 224ms for `uv lock`.

Prior to those PRs, I get 921ms for `rm uv.lock && uv lock` and 452ms for `uv lock`.

---

_Closed by @charliermarsh on 2024-06-24 21:09_

---
