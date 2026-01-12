```yaml
number: 436
title: Is there a way to make --files -g recurse into subdirectories?
type: issue
state: closed
author: jmatraszek
labels: []
assignees: []
created_at: 2017-04-02T21:49:37Z
updated_at: 2017-04-02T22:11:03Z
url: https://github.com/BurntSushi/ripgrep/issues/436
synced_at: 2026-01-12T16:13:22Z
```

# Is there a way to make --files -g recurse into subdirectories?

---

_@jmatraszek_

As suggested in #284 I am opening a new issue.

Using `linux` repo: there are almost 60k files there (`find | wc -l` reports 59 296).

`ag -g ''` reports 55 292 files and takes 0.8 second. This is the command that will be run by Unite plugin to collect the initial matches.

`rg --files -g *''*` reports 8 files (just the top-level files). So, let's try some other glob: `rg --files -g **/*''*` reports 965 (files inside top-level directories only). The only way to make `rg` return all files from the directory is to use `shopt -s globstar`, then `rg --files -g *''*` will return all files that should be returned, but it will take 3-4 seconds to run.

This wouldn't be that bad if Unite used this command on each pattern that is entered in its window (`ag -g 'Makefile'` and `rg --files -g *'Makefile'*` return (more or less) the same list of files). The problem is that it collects the initial list of files using this command, then just filters it for matches (so using `rg` only 8 files will be available to choose at the beginning and then, after typing `Makefile` as the pattern, one file: top-level Makefile).

So, is there a way to make `rg` recurse into directories (other than enabling `globstar`)?

P.S. Not a high-priority, I can always fallback to using `find`/`ag` in Unite. :) Thanks!

---

_Comment by @BurntSushi on 2017-04-02 21:55_

Why can't you just use `rg --files`?

---

_Comment by @jmatraszek on 2017-04-02 22:11_

Wow, this was so simple that I haven't thought of it! xD Thanks!

---

_Closed by @jmatraszek on 2017-04-02 22:11_

---
