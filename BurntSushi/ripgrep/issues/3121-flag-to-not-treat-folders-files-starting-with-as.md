```yaml
number: 3121
title: "Flag to *not* treat folders/files starting with `.` as hidden, but without other changes"
type: issue
state: closed
author: lgarron
labels:
  - wontfix
assignees: []
created_at: 2025-08-07T10:33:46Z
updated_at: 2025-08-08T00:45:30Z
url: https://github.com/BurntSushi/ripgrep/issues/3121
synced_at: 2026-01-12T16:13:25Z
```

# Flag to *not* treat folders/files starting with `.` as hidden, but without other changes

---

_@lgarron_

#### Describe your feature request

I use many files and folders that start with `.` (dot). I basically never want to consider these hidden, unless of course they are hidden by another rule (e.g. a `.gitignore`), and I keep being caught off-guard by ripgrep's default.

However, the `--hidden` re-includes too many things, such as `.git` directories. I can't figure out how to re-exclude these. For example, `--ignore-vcs` does not work.

I've spent a while now trying to piece together a command that excludes dot files from `--hidden` without other side effects, and I haven't figured it out yet. If this isn't supported, I'd be glad to see a feature for this.

---

_Comment by @BurntSushi on 2025-08-07 11:12_

Use `.rgignore` to whitelist what you want, or with `--hidden` use `.rgignore` to blacklist what you don't want.

ripgrep doesn't treat `.git` specially when it comes to filtering. It's just another hidden file/directory, and it will remain that way.

---

_Closed by @BurntSushi on 2025-08-07 11:12_

---

_Label `wontfix` added by @BurntSushi on 2025-08-07 11:12_

---

_Comment by @lgarron on 2025-08-08 00:13_

I see. That is not clear to me from `ripgrep --help`. I spent a long time assuming I was doing something wrong.

Would you be open to PR to spell that out a bit more clearly?

---

_Comment by @BurntSushi on 2025-08-08 00:45_

Sure.

---
