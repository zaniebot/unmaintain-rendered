---
number: 5183
title: "`uv tool list` might have a `--plain` flag to only list names?"
type: issue
state: open
author: baggiponte
labels:
  - needs-design
  - cli
assignees: []
created_at: 2024-07-18T12:39:09Z
updated_at: 2024-08-20T18:22:19Z
url: https://github.com/astral-sh/uv/issues/5183
synced_at: 2026-01-10T01:23:46Z
---

# `uv tool list` might have a `--plain` flag to only list names?

---

_Issue opened by @baggiponte on 2024-07-18 12:39_

This might be a request of a solution for the wrong problem. Since I cannot install (or upgrade) multiple tools with `uv tool`, I thought of using a bit of a hack:

```bash
for tool in $(uv tool list); do
    uv tool uninstall $tool && uv tool install $tool
done
```

However, `uv tool list` doesn't simply print all tools, but also their entrypoints. Maybe a `--plain` or `--tools-only` or `--no-entrypoints` flag might do?

I remember opening a similar feature request for `rye` to accept multiple packages at once, but Armin correctly pointed out the executable would not be able to handle the options/flags.

---

_Comment by @zanieb on 2024-07-18 14:22_

We're intending to address bulk operations some other way e.g. `uv tool upgrade --all`.

I wouldn't mind adding a `--no-show-executables` flag though; similar to how we just added a `--show-paths` flag in #5164 

---

_Label `good first issue` added by @zanieb on 2024-07-18 14:22_

---

_Comment by @zanieb on 2024-07-18 14:24_

Note I think you can also do `uv tool install <name> --upgrade` and skip the `uninstall`?

---

_Comment by @baggiponte on 2024-07-19 07:38_

> Note I think you can also do `uv tool install <name> --upgrade` and skip the `uninstall`?

Totally forgot about that. Thanks 😬

Feel free to close the issue whenever you want! And thanks again.

---

_Comment by @charliermarsh on 2024-07-19 13:22_

I still think `tool list --plain` or similar would need to include versions though.

---

_Label `cli` added by @charliermarsh on 2024-07-19 13:22_

---

_Label `preview` added by @charliermarsh on 2024-07-19 13:22_

---

_Comment by @zanieb on 2024-07-19 13:53_

Yeah idk, `--no-show-executables` and `--no-show-versions?`. Eventually it just makes sense to create a JSON output format.

---

_Label `good first issue` removed by @zanieb on 2024-07-19 13:53_

---

_Label `needs-design` added by @zanieb on 2024-07-19 13:53_

---

_Comment by @saaketp on 2024-08-12 16:49_

For comparison, [`pipx list`](https://pipx.pypa.io/stable/docs/#pipx-list) has `--short` to show only packages and versions without the entrypoints, and `--json` for json output.


---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---
