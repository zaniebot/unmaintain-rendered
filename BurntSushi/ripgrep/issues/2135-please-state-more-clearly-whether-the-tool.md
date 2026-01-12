```yaml
number: 2135
title: Please state more clearly whether the tool searches file names or file content
type: issue
state: closed
author: tempelmann
labels: []
assignees: []
created_at: 2022-01-26T21:27:29Z
updated_at: 2022-01-26T21:43:57Z
url: https://github.com/BurntSushi/ripgrep/issues/2135
synced_at: 2026-01-12T16:13:24Z
```

# Please state more clearly whether the tool searches file names or file content

---

_@tempelmann_

The initial readme says: "… searches the current directory for a regex pattern."

It is not clear to me _what_ it searches. The file names in the directory, or the text file content in the files in the directory.

It should be easy to fix that by editing that part of the sentence, thank you.

---

_Comment by @BurntSushi on 2022-01-26 21:43_

You left out the initial part of the sentence, which I think clarifies things:

> ripgrep is a *line-oriented* search tool

It also has "grep" in the name, which has a fairly ubiquitous meaning.

---

_Closed by @BurntSushi on 2022-01-26 21:43_

---
