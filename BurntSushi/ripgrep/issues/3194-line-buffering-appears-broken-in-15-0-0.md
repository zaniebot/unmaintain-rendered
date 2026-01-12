```yaml
number: 3194
title: Line buffering appears broken in 15.0.0
type: issue
state: closed
author: lespea
labels:
  - bug
assignees: []
created_at: 2025-10-18T23:09:14Z
updated_at: 2025-10-19T15:06:41Z
url: https://github.com/BurntSushi/ripgrep/issues/3194
synced_at: 2026-01-12T16:13:25Z
```

# Line buffering appears broken in 15.0.0

---

_@lespea_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 15.0.0

features:+pcre2
simd(compile):+SSE2,+SSSE3,+AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.45 is available (JIT is available)
```

### How did you install ripgrep?

Bug appears when built locally with Cargo as well as the binaries provided on the release page

### What operating system are you using ripgrep on?

Arch Linux -- kernel `6.17.3`

### Describe your bug.

I've been using ripgrep to filter `journalctl` output in follow mode for years and thus need the `--line-buffered` option otherwise nothing is output since `stdin` doesn't close.  With the latest release that switch doesn't appear to be having any affect.

### What are the steps to reproduce the behavior?

No output:

    journalctl -n5 -f | ./rg --no-config --line-buffered  'Oct'

Does produce some output -- I assume this is because the "block buffer" is getting full?

    journalctl -n5000 -f | ./rg --no-config --line-buffered  'Oct'

When run with < `15.0.0` each line that is matched is output as soon as it's printed by `journalctl`.  Obviously you'll have to adjust the search to match whatever's in your logs.

### What is the actual behavior?

Same as "steps to reproduce"

### What is the expected behavior?

Match lines (or inverse matches with `-v`) should be output as soon as they're output by the pipe

---

_Comment by @BurntSushi on 2025-10-19 13:38_

Ugh, yeah, this apparently broke in 8c6595c215d1e24bed5b7b86e2b18f3c871439ef (found via `git bisect`). No clue why yet though.

---

_Label `bug` added by @BurntSushi on 2025-10-19 13:38_

---

_Closed by @BurntSushi on 2025-10-19 15:06_

---
