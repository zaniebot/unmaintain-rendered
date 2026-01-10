---
number: 5758
title: "Show ephemeral progress bar in `uv run` and `uv tool run`"
type: issue
state: closed
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
created_at: 2024-08-04T12:28:35Z
updated_at: 2024-08-08T15:26:42Z
url: https://github.com/astral-sh/uv/issues/5758
synced_at: 2026-01-10T01:23:52Z
---

# Show ephemeral progress bar in `uv run` and `uv tool run`

---

_Issue opened by @charliermarsh on 2024-08-04 12:28_

When the environment _isn't_ cached / readily available, I think these now show _too little_ logging (for large environments, it feels like it's just hanging, when in reality it's preparing the environment). We should just show a spinner in those cases at least.

---

_Label `cli` added by @charliermarsh on 2024-08-04 12:28_

---

_Label `preview` added by @charliermarsh on 2024-08-04 12:28_

---

_Comment by @zanieb on 2024-08-05 14:24_

I guess `cargo run` isn't quiet by default, maybe we should reconsider? We should definitely show if we need to _build_ the root project.

---

_Comment by @charliermarsh on 2024-08-05 14:31_

Yeah it's too quiet right now... Some ideas (not mutually exclusive):

- Show an ephemeral spinner (so, it disappears once complete) if we have to do any work? Unfortunately we might have to "always" show this for `uv tool run` because (if the tool isn't already-installed) we have to perform a resolution even if we end up using a cached environment.
- Show the "Building" and "Built" outputs always? Like the cyan and green ones that appear separately from the progress spinners.
- Show the "Installed X packages in Yms" output but not the full list of modifications?


---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-05 19:53_

---

_Comment by @konstin on 2024-08-06 08:45_

Personally, i'd like `uv run` to be completely if everything is cached (i do `cargo run -q` often enough), show a spinner if something isn't and `Building`/`Built`/`Installed X packages in Yms` summary messages if something changes (but not the whole list of packages).

---

_Comment by @charliermarsh on 2024-08-06 12:24_

I agree. The main problem there is that with `uv tool run`, we have to resolve in order to locate a cached environment 🤔 

---

_Referenced in [astral-sh/uv#5899](../../astral-sh/uv/pulls/5899.md) on 2024-08-08 00:36_

---

_Closed by @charliermarsh on 2024-08-08 15:26_

---

_Closed by @charliermarsh on 2024-08-08 15:26_

---
