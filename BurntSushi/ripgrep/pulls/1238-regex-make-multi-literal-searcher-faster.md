```yaml
number: 1238
title: "regex: make multi-literal searcher faster"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/faster-literal-dictionary
created_at: 2019-04-07T22:45:01Z
updated_at: 2019-04-07T23:11:25Z
url: https://github.com/BurntSushi/ripgrep/pull/1238
synced_at: 2026-01-12T18:23:13Z
```

# regex: make multi-literal searcher faster

---

_@BurntSushi_

This makes the case of searching for a dictionary of a very large number
of literals much much faster. (~10x or so.) In particular, we achieve this
by short-circuiting the construction of a full regex when we know we have
a simple alternation of literals. Building the regex for a large dictionary
(>100,000 literals) turns out to be quite slow, even if it internally will
dispatch to Aho-Corasick.

Even that isn't quite enough. It turns out that even *parsing* such a regex
is quite slow. So when the -F/--fixed-strings flag is set, we short
circuit regex parsing completely and jump straight to Aho-Corasick.

We aren't quite as fast as GNU grep here, but it's much closer (less than
2x slower).

In general, this is somewhat of a hack. In particular, it seems plausible
that this optimization could be implemented entirely in the regex engine.
Unfortunately, the regex engine's internals are just not amenable to this
at all, so it would require a larger refactoring effort. For now, it's
good enough to add this fairly simple hack at a higher level.

Unfortunately, if you don't pass -F/--fixed-strings, then ripgrep will
be slower, because of the aforementioned missing optimization. Moreover,
passing flags like `-i` or `-S` will cause ripgrep to abandon this
optimization and fall back to something potentially much slower. Again,
this fix really needs to happen inside the regex engine, although we
might be able to special case -i when the input literals are pure ASCII
via Aho-Corasick's `ascii_case_insensitive`.

Fixes #497, Fixes #838

---

_Merged by @BurntSushi on 2019-04-07 23:11_

---

_Closed by @BurntSushi on 2019-04-07 23:11_

---

_Branch deleted on 2019-04-07 23:11_

---
