```yaml
number: 1963
title: "Feature: --ignored-files, the inverse of --files"
type: issue
state: closed
author: zachriggle
labels:
  - wontfix
assignees: []
created_at: 2021-08-03T18:17:52Z
updated_at: 2021-08-03T18:27:22Z
url: https://github.com/BurntSushi/ripgrep/issues/1963
synced_at: 2026-01-12T16:13:24Z
```

# Feature: --ignored-files, the inverse of --files

---

_@zachriggle_

As always, thanks @BurntSushi for making RipGrep.  It's made my life immeasurably better and probably saved many, many hours waiting for other, slower grep tools.  Please consider adding a "Sponsor" button to the repo, so I can buy you a beer (https://github.com/sponsors).

One thing that is useful sometimes is the `--files` flag, so I can see what files would be searched (i.e. those which are not ignored by any e.g. `.gitignore` or `.rgignore` or other ignore files).

It would be nice to have the inverse of this, perhaps `--ignored-files`, which prints out all files that are NOT searched.

My specific use-case is a tool that wraps `rg`, where the user may have specified a custom ignore file or have a `.rgignore` file.  When there are NO matches for a given search, I'd like some way to be able to inform the user what files were skipped.

---

_Comment by @BurntSushi on 2021-08-03 18:24_

> It would be nice to have the inverse of this, perhaps --ignored-files, which prints out all files that are NOT searched.

I think it would be better to just implement this outside of ripgrep since you need this in the context of a wrapper tool. It's easy to do with standard Unix tooling:

```
$ rg --files | sort > /tmp/search-yes
$ rg --files -uuu | sort > /tmp/search-all
$ comm -13 /tmp/search-yes /tmp/search-all > /tmp/search-no
```

In contrast, modifying ripgrep to support the proposed option is actually quite difficult. Most of the directory traversal code is tightly coupled with the assumption that we only care about files that match the ignore rules. Thus, implementing this feature requires de-coupling that assumption and therefore making the code more complex. It's just not worth it. Sorry.

> Please consider adding a "Sponsor" button to the repo, so I can buy you a beer (https://github.com/sponsors).

I appreciate the thought. See the FAQ: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#donations


---

_Closed by @BurntSushi on 2021-08-03 18:24_

---

_Label `wontfix` added by @BurntSushi on 2021-08-03 18:24_

---

_Comment by @zachriggle on 2021-08-03 18:27_

That makes sense!  Since this is not a hot code path / common scenario, dumping the output to disk and just `diff`'ing (or `comm`'ing) is an easy enough fix.  Thanks for the consideration!

_(Side bar: I just learned about `comm` earlier today, neat to see it show up this afternoon!)_

---
