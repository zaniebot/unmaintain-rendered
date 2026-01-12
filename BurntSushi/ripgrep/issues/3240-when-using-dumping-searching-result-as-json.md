```yaml
number: 3240
title: "When using dumping searching result as json format, the submatch \"end\" field is not correct."
type: issue
state: closed
author: 0-EricZhou-0
labels: []
assignees: []
created_at: 2025-12-09T07:23:07Z
updated_at: 2025-12-09T13:06:34Z
url: https://github.com/BurntSushi/ripgrep/issues/3240
synced_at: 2026-01-12T16:13:25Z
```

# When using dumping searching result as json format, the submatch "end" field is not correct.

---

_@0-EricZhou-0_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 15.1.0 (rev af60c2de9d)

### How did you install ripgrep?

Download precompiled binary from release

### What operating system are you using ripgrep on?

Ubuntu 24.04.2 LTS and Ubuntu 22.04.5 LTS in wsl

### Describe your bug.

When using `ripgrep` with `--json` flag, in the submatch fields, the `["data"]["submatches"]["end"]` field can be wrong. However, the `["data"]["submatches"]["start"]` and `["data"]["submatches"]["match"]["text"]` is correct, and we can get the correct submatch end offset using submatch start offset + submatch text length.

Note that the file to search is a log file containing multiple `\CR` (`^M`) and `\EC` (`^[`) characters, and `--multiline` is enabled for the search but I think there is nothing to do with it.

### What are the steps to reproduce the behavior?

See https://github.com/0-EricZhou-0/ripgrep-bug-report, I have made a minimum example to repoduce the issue.

### What is the actual behavior?

Submatch end is at wrong index and could go beyond the match text length.

### What is the expected behavior?

Submatch end should be at correct index.

---

_Comment by @0-EricZhou-0 on 2025-12-09 08:27_

Seems to be a encoding issue, `python`'s `json` module will autometically do decode for me. I need to encode the strings back to bytes to get the correct length and offset.

---

_Closed by @0-EricZhou-0 on 2025-12-09 08:27_

---

_Locked by @ghost on 2025-12-09 13:06_

---
