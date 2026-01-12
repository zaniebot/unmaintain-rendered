```yaml
number: 1080
title: "Ripgrep does not support ** in .gitignore files"
type: issue
state: closed
author: RedX2501
labels:
  - duplicate
assignees: []
created_at: 2018-10-08T15:16:40Z
updated_at: 2018-10-09T07:15:59Z
url: https://github.com/BurntSushi/ripgrep/issues/1080
synced_at: 2026-01-12T16:13:22Z
```

# Ripgrep does not support ** in .gitignore files

---

_@RedX2501_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)

#### How did you install ripgrep?

Github binary release

#### What operating system are you using ripgrep on?

Windows within bash provided with git (msys environment)

#### Describe your question, feature request, or bug.

In windows `.gitconfig` can contains `**` in paths. Example:

    doc**/

#### If this is a bug, what are the steps to reproduce the behavior?

Run command

    rg unfindable

#### If this is a bug, what is the actual behavior?

Currently I get the following error:

    ./.git/info/exclude: line 12: error parsing glob 'doc**/': invalid use of **; must be one path component
    ./.git/info/exclude: line 13: error parsing glob 'Release**/': invalid use of **; must be one path component


#### If this is a bug, what is the expected behavior?

Support this pattern (at least on windows)

This pattern is even (documented)[https://git-scm.com/docs/gitignore#_pattern_format]


---

_Comment by @BurntSushi on 2018-10-08 15:24_

ripgrep **does** support `**`. The error message is telling you exactly what the problem is: use of `**` must be one path component. Indeed, the documentation you linked to explicitly says this:

> * A leading `**` followed by a slash means match in all directories. For example, `**/foo` matches file or directory `foo` anywhere, the same as pattern `foo`. `**/foo/bar` matches file or directory `bar` anywhere that is directly under directory `foo`.
>
> * A trailing `/**` matches everything inside. For example, `abc/**` matches all files inside directory `abc`, relative to the location of the `.gitignore` file, with infinite depth.
>
> * A slash followed by two consecutive asterisks then a slash matches zero or more directories. For example, `a/**/b` matches `a/b`, `a/x/b`, `a/x/y/b` and so on.
>
> * **Other consecutive asterisks are considered invalid.**

Otherwise, this is a duplicate of #373.

---

_Closed by @BurntSushi on 2018-10-08 15:24_

---

_Label `duplicate` added by @BurntSushi on 2018-10-08 15:24_

---

_Comment by @RedX2501 on 2018-10-09 07:15_

I completely agree with you. Yet git does not complain on windows. I'll see if chaning the pattern produces the same result.

---
