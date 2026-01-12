```yaml
number: 2015
title: "Option to match pattern to both files' content and their paths"
type: issue
state: closed
author: besfahbod
labels:
  - wontfix
assignees: []
created_at: 2021-10-11T19:21:17Z
updated_at: 2023-11-24T20:14:34Z
url: https://github.com/BurntSushi/ripgrep/issues/2015
synced_at: 2026-01-12T16:13:24Z
```

# Option to match pattern to both files' content and their paths

---

_@besfahbod_

#### Describe your feature request

It is common in some code-bases to have a file-name being the identifier of a feature, which could be of interest of someone looking for code of that feature. Sometimes, these identifiers are not repeated inside the file content, causing a regular "grep" action on the code-base to fail.

An example of this would be a library using the [requireindex npm package](https://www.npmjs.com/package/requireindex) to have submodules loaded without having to repeat the module names. Then, having the identifier of each of those modules (the file name), it's not possible to find these modules using a regular grep (unless the name is repeated somewhere, which is not always the case).

We could argue that it's a better job for `git` to implement such a feature, and I would agree that git also could use a combination of its `grep` and `lf` subcommands. However, `rg`, with the existing great support for `git` repositories, could also use a feature.

Afaik, ripgrep is not currently expected to be used for matching file names as a primary objective. Meaning, if we're going to add a content+path search feature, it probably makes sense to *also* have a path-only search feature added. This could be considered out-of-the-scope for the project, of course. However, this would make ripgrep also a good replacement for `find`/`git lf`, in addition to the current content searching feature—without too much extra cost, as the existing code is already walking over dirs/files and does have some path-matching rules in place.

What do y'all think?

---

_Comment by @BurntSushi on 2021-10-12 12:11_

Dupe of #1002, #1034 and #1901. [#1034 contains some of my more specific concerns.](https://github.com/BurntSushi/ripgrep/issues/1034#issuecomment-417968082)

I really think the simpler answer here is, if you need it, to write a wrapper script that does both searches for you.

---

_Comment by @BurntSushi on 2023-11-24 20:14_

I haven't changed my mind on this in the intervening years, so I'm gong to close this.

---

_Closed by @BurntSushi on 2023-11-24 20:14_

---

_Label `wontfix` added by @BurntSushi on 2023-11-24 20:14_

---
