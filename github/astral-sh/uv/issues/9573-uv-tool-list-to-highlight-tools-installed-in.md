---
number: 9573
title: "\"uv tool list\" to highlight tools installed in editable mode"
type: issue
state: open
author: afaucon
labels:
  - enhancement
assignees: []
created_at: 2024-12-02T11:50:48Z
updated_at: 2024-12-04T16:54:02Z
url: https://github.com/astral-sh/uv/issues/9573
synced_at: 2026-01-07T13:12:18-06:00
---

# "uv tool list" to highlight tools installed in editable mode

---

_Issue opened by @afaucon on 2024-12-02 11:50_

Dear uv community and core team,

Congratulation for your job.

This is a question for a feature as I did not find any info in the documentation neither in existing issues.
I installed a tool in editable mode with `uv tool install -e .`
To list the installed tools: `uv tool list`.
But in the list of installed tools, how can I know which one have been installed in editable mode, and where are the source coe location?

BR,
Adrien

---

_Label `enhancement` added by @charliermarsh on 2024-12-02 13:24_

---

_Comment by @charliermarsh on 2024-12-02 13:25_

Ah yeah, this isn't displayed today. We could consider showing it.

---

_Comment by @zanieb on 2024-12-02 15:55_

Thinking about this interface.. 

```
❯ uv tool list
rooster-blue v0.0.0
- rooster
```

could become

```
❯ uv tool list
rooster-blue v0.0.0 (from /Users/zb/workspace/rooster)
- rooster
```

for local sources? Perhaps we'd want a dedicated message for editable installs?

```
❯ uv tool list
rooster-blue v0.0.0 (editable from /Users/zb/workspace/rooster)
- rooster
```

There's a slight conflict with `--show-paths`:

```
❯ uv tool list --show-paths
rooster-blue v0.0.0 (/Users/zb/.local/share/uv/tools/rooster-blue)
- rooster (/Users/zb/.local/bin/rooster)
```

It seems ok to duplicate the parenthesis since it's not a common display format.

```
❯ uv tool list --show-paths
rooster-blue v0.0.0 (editable from /Users/zb/workspace/rooster) (/Users/zb/.local/share/uv/tools/rooster-blue)
- rooster (/Users/zb/.local/bin/rooster)
```

---

_Comment by @j178 on 2024-12-04 12:27_

 While working on #9636, the `uv tool list` is generating more and more information, making it difficult to fit everything on one line. I am considering changing the format to something like this:

```console
❯ uv tool list --show-paths --show-version-specifiers --outdated
rooster-blue v0.0.0
  latest:      v0.0.1
  required:    <1
  editable:    /Users/zb/workspace/rooster
  path:        /Users/zb/.local/share/uv/tools/rooster-blue
  entrypoints:
    - rooster (/Users/zb/.local/bin/rooster)
```

---

_Comment by @zanieb on 2024-12-04 16:54_

Hm. It's hard to say if we should be optimizing for the "show everything" use-case. That seems like a better fit for `uv tool list --output-format json`. Like, why would you need `--show-paths` with `--outdated`?



---
