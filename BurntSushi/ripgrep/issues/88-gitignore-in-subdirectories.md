```yaml
number: 88
title: "`.gitignore` in subdirectories"
type: issue
state: closed
author: alexlafroscia
labels: []
assignees: []
created_at: 2016-09-25T19:34:20Z
updated_at: 2016-09-25T20:25:36Z
url: https://github.com/BurntSushi/ripgrep/issues/88
synced_at: 2026-01-12T18:23:11Z
```

# `.gitignore` in subdirectories

---

_@alexlafroscia_

As far as I can tell, `ripgrep` doesn't pick up `.gitignore` files defined in subdirectories.  For example, I have a Git repo that has a `.gitignore` in the root of the project and then another one in `src/frontend` relative to the root.  Because the ignore file in the subdirectory is not used, I end up with search results from all kinds of temporary and output files that should be ignored.

I'd imagine that, from a performance standpoint, this is probably the right behavior so feel free to close this if that's the case 😄


---

_Comment by @BurntSushi on 2016-09-25 19:37_

It absolutely picks up `gitignore` files in subdirectories.

There is likely a bug impacting one or more patterns. I've fixed several in the last day or two, but haven't cut a new release yet. If you can test the latest master, that'd be helpful. But otherwise, including a reproducible test case would be a huge help.


---

_Comment by @alexlafroscia on 2016-09-25 19:41_

Sure, will do!


---

_Comment by @alexlafroscia on 2016-09-25 20:17_

The code on `master` fixes my issue.  If I do `rg --files | grep tmp` with the fixes you mentioned, I see no matches, but did before.  Thanks for the help!


---

_Closed by @alexlafroscia on 2016-09-25 20:17_

---

_Comment by @BurntSushi on 2016-09-25 20:25_

I love bugs that are already fixed. :-)

I'll be getting a new release out shortly!


---
