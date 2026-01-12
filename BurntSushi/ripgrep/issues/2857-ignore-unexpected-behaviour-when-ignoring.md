```yaml
number: 2857
title: "[ignore] Unexpected behaviour when ignoring directories without a slash"
type: issue
state: closed
author: lbirkert
labels: []
assignees: []
created_at: 2024-07-18T19:35:02Z
updated_at: 2024-07-19T10:58:19Z
url: https://github.com/BurntSushi/ripgrep/issues/2857
synced_at: 2026-01-12T16:13:25Z
```

# [ignore] Unexpected behaviour when ignoring directories without a slash

---

_@lbirkert_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

using the library ignore = "0.4.22"

### How did you install ripgrep?

using the library ignore = "0.4.22"

### What operating system are you using ripgrep on?

Linux

### Describe your bug.

Ripgreps ignore matching does not behave equally to gits ignore matching.

### What are the steps to reproduce the behavior?

-

### What is the actual behavior?

.gitignore: `my_dir` or `my_dir/`
matches `my_dir` itself but does not match `my_dir/**`

### What is the expected behavior?

.gitignore: `my_dir` or `my_dir/`
should match `my_dir` and `my_dir/**`

This however is not how git handles gitignore matches and should be resolved.

---

_Comment by @BurntSushi on 2024-07-18 19:59_

Please provide an MRE.

---

_Comment by @lbirkert on 2024-07-19 10:56_

made this repo https://github.com/lbirkert/ripgrep-issue-2857

```
git clone https://github.com/lbirkert/ripgrep-issue-2857
cd ripgrep-issue-2857
cargo run
```

---

_Comment by @lbirkert on 2024-07-19 10:58_

it seems like I have used the wrong method here ups.
Should've used "matched_path_or_any_parents" instead.

---

_Closed by @lbirkert on 2024-07-19 10:58_

---
