```yaml
number: 9230
title: "Build backend: Build editable "
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-editables
created_at: 2024-11-19T15:46:50Z
updated_at: 2024-11-19T21:52:12Z
url: https://github.com/astral-sh/uv/pull/9230
synced_at: 2026-01-10T12:00:00Z
```

# Build backend: Build editable 

---

_Pull request opened by @konstin on 2024-11-19 15:46_

Support for editable installs. This is a simple PEP 660 implementation.

---

_Label `preview` added by @konstin on 2024-11-19 15:46_

---

_Review requested from @BurntSushi by @konstin on 2024-11-19 15:46_

---

_Comment by @konstin on 2024-11-19 16:19_

I'm stripping the pytest run:

![image](https://github.com/user-attachments/assets/5816a035-3520-41e8-b06b-d96fc02c31b7)


---

_Review comment by @BurntSushi on `crates/uv/src/commands/build_backend.rs`:43 on 2024-11-19 16:58_

`println!` has an unavoidable panic when writing to stdout fails. I would suggest `writeln!(&mut std::io::stdout(), "{filename}")?` here instead.

---

_Review comment by @BurntSushi on `crates/uv/src/commands/build_backend.rs`:43 on 2024-11-19 17:00_

Is it definitely intended for this function to just write to stdout? If so, I might put that in the docs for the function. It's hard to articulate why, but it does seem somewhat surprising. I think part of it is that I don't quite understand why `filename` is being written to stdout. Is that just part of PEP 660? (I'm not familiar with it.)

---

_Review comment by @BurntSushi on `crates/uv/src/commands/build_backend.rs`:78 on 2024-11-19 17:00_

Same comment here as above.

---

_@BurntSushi approved on 2024-11-19 17:01_

Some minor comments/nits, but LGTM!

---

_@konstin reviewed on 2024-11-19 21:40_

---

_Review comment by @konstin on `crates/uv/src/commands/build_backend.rs`:43 on 2024-11-19 21:40_

It's the way in PEP 517 and PEP 660 how the build backend tells the frontend where to find the build artifact: It's in `<wheel_directory>/<last line of stdout>`.

I'm adding references to the spec name in the doc comment, otherwise I'm assuming familiarity with the spec (sorry - i don't expect you to read them and will of course answer questions for you). I tried to copy all relevant excerpts from the spec in earlier PEP implementations I did, but didn't found it to be effective.

---

_Merged by @konstin on 2024-11-19 21:52_

---

_Closed by @konstin on 2024-11-19 21:52_

---

_Branch deleted on 2024-11-19 21:52_

---
