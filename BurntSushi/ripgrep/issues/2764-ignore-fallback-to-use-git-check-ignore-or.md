```yaml
number: 2764
title: "[ignore] Fallback to use git check-ignore or support hook iteration"
type: issue
state: closed
author: tisonkun
labels: []
assignees: []
created_at: 2024-03-24T04:29:54Z
updated_at: 2024-03-24T05:28:19Z
url: https://github.com/BurntSushi/ripgrep/issues/2764
synced_at: 2026-01-12T16:13:24Z
```

# [ignore] Fallback to use git check-ignore or support hook iteration

---

_@tisonkun_

Since ignore is unaware of git index, there is a pitfall that a file force added to Git repository is ignored.

Currently, I write the logic below to switch the manner:

```rust
let walker = ignore::WalkBuilder::new(&self.basedir)
            .ignore(false) // do not use .ignore file
            .hidden(false) // check hidden files
            .follow_links(true) // proper path name
            .parents(turn_on_ignore)
            .git_exclude(turn_on_ignore)
            .git_global(turn_on_ignore)
            .git_ignore(turn_on_ignore)
            .overrides( ... );

for mat in walker {
  ...
                if let Some(helper) = git_helper.as_ref()
                    && helper.ignored(mat.path())?
                {
                    continue;
                }
                result.push(mat.into_path())
}
```

where ignored is a wrapper of `git2::Repository::is_path_ignored`.

However, this solution has a shortcoming that the walker iteration is determinated unaware of the Git ignore result, so it cannot skip directory ignored but must process all the files.

So I propose add an option to fallback to use git check-ignore or support hook iteration.

cc @BurntSushi 

---

_Comment by @tisonkun on 2024-03-24 04:30_

Source code is publicly available at -

https://github.com/korandoru/hawkeye/blob/v5.0.0/fmt/src/selection.rs#L99-L136
https://github.com/korandoru/hawkeye/blob/v5.0.0/fmt/src/git.rs#L65-L70

---

_Comment by @tisonkun on 2024-03-24 04:39_

Or I may use `Override` directly and do a manual walk through. I don't give it a try though.

---

_Comment by @tisonkun on 2024-03-24 05:28_

Try to use a bare `Override` with `walkdir` crate. So far so good - https://github.com/korandoru/hawkeye/commit/e7be36cfb72f8ad663039fe4360a8a8aa4e05a9f

---

_Closed by @tisonkun on 2024-03-24 05:28_

---
