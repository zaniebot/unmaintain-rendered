```yaml
number: 5246
title: "Add `uv add --no-editable`"
type: pull_request
state: merged
author: j178
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: editable
created_at: 2024-07-20T12:37:19Z
updated_at: 2024-07-22T18:42:45Z
url: https://github.com/astral-sh/uv/pull/5246
synced_at: 2026-01-10T13:37:23Z
```

# Add `uv add --no-editable`

---

_Pull request opened by @j178 on 2024-07-20 12:37_

## Summary

Resolves #5241

## Test Plan

```sh
# create a workspace with sub-packages `pkg-a` and `pkg-b`

$ cd ./pkg-b
$ cargo run -- add ./pkg-a --no-editable

$ cat ./pyproject.toml
[project]
name = "pkg-b"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = [
    "pkg-a",
]

[tool.uv]
dev-dependencies = []

[tool.uv.sources]
pkg-a = { workspace = true, editable = false }
```

---

_Label `cli` added by @charliermarsh on 2024-07-20 13:11_

---

_Label `preview` added by @charliermarsh on 2024-07-20 13:11_

---

_Merged by @charliermarsh on 2024-07-20 13:11_

---

_Closed by @charliermarsh on 2024-07-20 13:11_

---

_Comment by @zanieb on 2024-07-20 15:51_

@charliermarsh this was implemented intentionally as `--editable` and `--editable=true` / `--editable=false` to match the syntax you'd use in the pyproject.toml. I don't prefer `--no-editable`.

cc @ibraheemdev 

---

_Comment by @charliermarsh on 2024-07-20 15:55_

I don't understand why you would use `--editable=true`, that's so surprising!

---

_Comment by @charliermarsh on 2024-07-20 15:58_

Oh, you're saying `--editable` already worked, but it _also_ supported `--editable=true` and `--editable=false`? I don't know, that's also not as intuitive to me given that we don't have that pattern anywhere else in the CLI.

---

_Comment by @zanieb on 2024-07-20 16:08_

Yeah because of `default_missing_value = "true"`.

I know it's not aligned with the rest of the CLI — but it does directly match the `sources` definition. I don't know if I feel particularly strongly, but wanted to flag that this wasn't an oversight. I'm not sure why, but `--no-editable` doesn't feel like it encodes the meaning as clearly (although it is more intuitive given our _existing_ patterns).

---

_Comment by @charliermarsh on 2024-07-20 16:09_

Ok I defer to others on this (you and @ibraheemdev). I also don't feel strongly. I thought the original report was that you had to do `--editable=true` which I knew every user would get wrong :)

---

_Comment by @zanieb on 2024-07-20 16:12_

Maybe the most user-friendly thing is to support `--editable` / `--no-editable` and `--editable=true` / `--editable=false`. It's not like there's something else we'll want the flag for. 

Are there other booleans that would make sense as `--<name>=false` (instead of `--no-<name>`)?

I'll think on this. Unsure if we should revert in the meantime.

---

_Branch deleted on 2024-07-22 14:21_

---

_Comment by @bluss on 2024-07-22 18:10_

Sorry for this one then.

I'll chime in and say the reason I reported the bug was this interaction (uv 0.2.24):

```
> uv add --editable ./sibling/
error: invalid value './sibling/' for '--editable [<EDITABLE>]'
```

So just `--editable` worked in some places but not before the name of the dependency.

---

_Comment by @charliermarsh on 2024-07-22 18:12_

Ahh yeah. That's actually a pretty big footgun in my opinion.

---

_Comment by @zanieb on 2024-07-22 18:42_

Ah okay sold. Thanks!

---
