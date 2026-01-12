```yaml
number: 3147
title: "cli: --iglob filter paths in argv"
type: pull_request
state: closed
author: phanen
labels: []
assignees: []
base: master
head: iglob-paths
created_at: 2025-09-13T12:35:49Z
updated_at: 2025-09-13T12:42:46Z
url: https://github.com/BurntSushi/ripgrep/pull/3147
synced_at: 2026-01-12T18:23:15Z
```

# cli: --iglob filter paths in argv

---

_@phanen_

`--{,i}glob` don't filter path in argv

can we add an option to support it or (this pr make it default now):
```sh
$ cargo run -- -e r LICENSE-MIT README.md --iglob '!*.md' --files
LICENSE-MIT
$ cargo run -- -e r LICENSE-MIT README.md --iglob '*.md' --files
README.md
```

For me paths in argv are built from fzf output, then I can use `--iglob` to filter the result in fzf live query again.


---

_Comment by @BurntSushi on 2025-09-13 12:42_

No, this is an intentional design. It is very purposeful that if you pass a path to ripgrep, then ripgrep will always search it _regardless_ of any kind of filtering. To do otherwise would be madness. It is a useful property to be able to say, "if you give a file to ripgrep, it will get searched, no matter what."

This means that if you want filtering like this, it must be done before passing the file paths to ripgrep.

---

_Closed by @BurntSushi on 2025-09-13 12:42_

---
