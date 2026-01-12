```yaml
number: 5967
title: "docs: combine NOTE lines that were accidentally split"
type: pull_request
state: merged
author: eric-seppanen
labels: []
assignees: []
merged: true
base: master
head: args_raw_doc_fix
created_at: 2025-04-08T22:32:44Z
updated_at: 2025-04-08T22:55:24Z
url: https://github.com/clap-rs/clap/pull/5967
synced_at: 2026-01-12T16:14:17Z
```

# docs: combine NOTE lines that were accidentally split

---

_@eric-seppanen_

Commit 6f78e2a3ff01 split apart the lines of the note, which looks like a mistake.

The documentation looks a little jumbled as a result: https://docs.rs/clap/4.5.35/clap/struct.Arg.html#method.raw

I also added a comma between the first two items.


---
