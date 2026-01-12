```yaml
number: 2928
title: "`resolve_binary` name is misleading"
type: issue
state: closed
author: dandavison
labels:
  - doc
assignees: []
created_at: 2024-11-10T15:46:53Z
updated_at: 2025-09-23T01:38:49Z
url: https://github.com/BurntSushi/ripgrep/issues/2928
synced_at: 2026-01-12T16:13:25Z
```

# `resolve_binary` name is misleading

---

_@dandavison_

Hi, thanks again for ripgrep! In delta we're using [`resolve_binary`](https://docs.rs/grep-cli/latest/grep_cli/fn.resolve_binary.html), and I have a question / feature request regarding its API (which I'm opening as a feature request issue). The second sentence of the docstring says `"If the program could not be resolved, then an error is returned."`, whereas in fact this is only true on Windows, as the last sentence says. This has caused me and others to make mistakes. My question is

- Would a name like `resolve_windows_binary` have been a clearer name, or was there a reason not to do that?

And my feature request

- Assuming a breaking renaming change is not reasonable, how about moving the final sentence of the docstring (`"On non-Windows, this is a no-op."`) up so that it becomes part of the first sentence of the docstring?

---

_Closed by @BurntSushi on 2025-09-23 01:38_

---

_Comment by @BurntSushi on 2025-09-23 01:38_

I added a `# Platform behavior` section to try and address this.

---

_Label `doc` added by @BurntSushi on 2025-09-23 01:38_

---
