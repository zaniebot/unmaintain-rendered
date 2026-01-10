```yaml
number: 8301
title: "Pipe input directly in `uv run` command"
type: pull_request
state: closed
author: Aditya-PS-05
labels: []
assignees: []
base: main
head: performance/6529-uv-run
created_at: 2024-10-17T18:34:23Z
updated_at: 2025-09-11T22:52:37Z
url: https://github.com/astral-sh/uv/pull/8301
synced_at: 2026-01-10T06:36:15Z
```

# Pipe input directly in `uv run` command

---

_Pull request opened by @Aditya-PS-05 on 2024-10-17 18:34_

closes #6529

## Summary
Locally tested, the expected behavior works but some tests are being failed.


---

_Comment by @zanieb on 2024-10-17 19:01_

Can you title the pull request as a description of the user-facing change? (i.e., as things appear in our changelog)

---

_Renamed from "Performance/6529 uv run" to "Pipe input directly in `uv run` command" by @Aditya-PS-05 on 2024-10-17 19:13_

---

_Comment by @Aditya-PS-05 on 2024-10-18 13:41_

@zanieb , 
Title changed and please review it.

---

_Comment by @charliermarsh on 2024-10-22 02:10_

I'm hesitant to make this change because we still have to read the entire file ourselves in order to find the PEP 723 metadata from the script (which can appear anywhere in the script).


---

_Comment by @Aditya-PS-05 on 2024-10-22 05:14_

@charliermarsh , 
Can you suggest an alternate good method to be implemented? 

---

_Comment by @zanieb on 2024-10-22 11:36_

I'm not sure I agree @charliermarsh, I think it'll be very rare for the metadata to not be at the head of the file and I think it's far more likely that we read the whole file into memory unnecessarily and degrade performance in that way. It's possible the additional complexity here isn't justified, as nobody as complained yet — but I don't think "we could need to read the whole file anyway" is a particularly compelling point against implementing this.

---

_Comment by @charliermarsh on 2024-10-22 11:44_

It’s not that we _could_ need to read the whole file into memory. It’s that we _have_ to scan the whole file, every time, _unless_ it’s actually a PEP 723 script with a comment at the top. Otherwise, we can’t know that the file _isn’t_ a PEP 723 script until we’ve read the whole thing, and we can’t execute it until we know whether it’s a PEP 723 script.

---

_Comment by @zanieb on 2024-10-22 11:48_

Ah I see. That's a great point. We could just say we don't support inferring that something is a PEP 723 script if it's not in the header, but I see why that's also not appealing given that nobody is complaining about us reading the script into memory today.

---

_Comment by @charliermarsh on 2024-10-22 11:50_

You’re totally right though that if the file is a PEP 723 script and the comment is near the top, we can just buffer until we see it, then steam the rest (and avoid reading the file into memory). So if a lot of the usages here are PEP 723 scripts (which almost certainly have the header near the top), then it would make a difference.

---

_Comment by @charliermarsh on 2024-10-22 11:50_

In that light, I guess I’m not objecting to the behavior in principle, more that it will be complex :)

---

_Closed by @Aditya-PS-05 on 2025-09-11 22:52_

---
