```yaml
number: 7457
title: Documented uv run python to Provide REPL Access
type: pull_request
state: closed
author: Aditya-PS-05
labels:
  - documentation
assignees: []
base: main
head: docs/clarify-uv-run-python-repl
created_at: 2024-09-17T13:18:43Z
updated_at: 2025-09-11T22:52:37Z
url: https://github.com/astral-sh/uv/pull/7457
synced_at: 2026-01-10T06:36:15Z
```

# Documented uv run python to Provide REPL Access

---

_Pull request opened by @Aditya-PS-05 on 2024-09-17 13:18_

closes #6548

## Summary
Updated the docs and included the uv run python repl section.


---

_Review comment by @lucab on `docs/guides/scripts.md`:16 on 2024-09-18 06:33_

Leftover comment that needs to be removed?

---

_Review comment by @lucab on `docs/guides/scripts.md`:33 on 2024-09-18 06:36_

```suggestion
This will open a Python session in interactive mode, where you can execute individual Python commands and experiment
with the environment.
```

---

_@lucab reviewed on 2024-09-18 06:36_

---

_Label `documentation` added by @lucab on 2024-09-18 06:41_

---

_@Aditya-PS-05 reviewed on 2024-09-18 07:41_

---

_Review comment by @Aditya-PS-05 on `docs/guides/scripts.md`:33 on 2024-09-18 07:41_

@lucab 
Thank you for valuable suggestion.

---

_Comment by @Aditya-PS-05 on 2024-10-16 17:57_

@lucab , 
If this solves the issue, can you merge the pr?

---

_Comment by @Aditya-PS-05 on 2025-01-13 16:14_

@lucab, please review it.

---

_Comment by @zanieb on 2025-01-13 18:23_

I don't think this belongs in the "Scripts" guide. Is there a reason you put it here?

---

_Renamed from "added uv run python section" to "Documented uv run python to Provide REPL Access" by @Aditya-PS-05 on 2025-01-14 18:06_

---

_Comment by @Aditya-PS-05 on 2025-01-14 18:11_

> I don't think this belongs in the "Scripts" guide. Is there a reason you put it here?

Since the guide/scripts.md already covers related topics, I thought it would be a logical place.


---

_Comment by @andrei-korshikov on 2025-03-17 14:00_

> I don't think this belongs in the "Scripts" guide. Is there a reason you put it here?

I do agree that "Scripts" is a strange place. To my understanding, we have four distinct "runnable" things: arbitrary command (including Python itself), script, project, "tool" (including Python once again which adds a bit of confusion).

My suggestion is: "First steps", "Features" and "Commands".

* [First steps](https://docs.astral.sh/uv/getting-started/first-steps/)—put a subsection with prose about running REPL here.
* [Features](https://docs.astral.sh/uv/getting-started/features/)—add a subsection e.g. between [Projects](https://docs.astral.sh/uv/getting-started/features/#projects) and [Tools](https://docs.astral.sh/uv/getting-started/features/#tools) and mention `uv run python`. I think such subsection should not be dedicated to REPL only, but something like "Hey, you can run anything including interpreter". And maybe mention other good samples of _anything_ if any.
* [Commands](https://docs.astral.sh/uv/reference/cli/)—mention running REPL with `uv run python` in the intro of [uv run](https://docs.astral.sh/uv/reference/cli/#uv-run). Yes, it contains `uv run --python 3.12 -- python` sample, but it's about `--` usage.

---

_Closed by @Aditya-PS-05 on 2025-09-11 22:52_

---
