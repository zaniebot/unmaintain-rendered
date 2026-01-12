```yaml
number: 1092
title: "What .gitignore is considered? [question]"
type: issue
state: closed
author: AdamRGrey
labels: []
assignees: []
created_at: 2018-10-26T13:10:02Z
updated_at: 2018-10-26T13:19:00Z
url: https://github.com/BurntSushi/ripgrep/issues/1092
synced_at: 2026-01-12T16:13:22Z
```

# What .gitignore is considered? [question]

---

_@AdamRGrey_

#### What version of ripgrep are you using?
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

choco install ripgrep

#### What operating system are you using ripgrep on?

windows 7 SP1

#### Describe your question, feature request, or bug.

How does ripgrep actually understand .gitignore files? Suppose I'm in directory foo:

```
foo/
| - bar/
| --- .gitignore
| --- file.txt
| - baz/
| --- other file.txt
```
if I run ripgrep from foo, does that mean it will understand the .gitignore within bar, and if for example file.txt is listed in that .gitignore, it would ignore that file? I'm guessing it won't apply the .gitignore inside baz.

Or, do I have to specify a .gitignore somehow? 

Or, does it only look inside the directory where it was run, and if it doesn't find one, doesn't ever use one?

---

_Comment by @BurntSushi on 2018-10-26 13:14_

If you're in `foo`, it will respect the `.gitignore` in `bar`. Generally speaking, a `.gitignore` file can only apply to files in its directory or lower.

Basically, ripgrep treats `.gitignore` files just like git does. So if you know how `.gitignore` works with git, then you know how `.gitignore` works with ripgrep automatically. There are some bugs that lead to a few inconsistencies, but they are the exception, not the rule.

---

_Comment by @BurntSushi on 2018-10-26 13:14_

If you have a specific example of something that isn't working like you expect, then it might be better to just show that.

---

_Comment by @AdamRGrey on 2018-10-26 13:17_

No bug, just asking.

Thanks, that clarifies it.

---

_Closed by @BurntSushi on 2018-10-26 13:19_

---
