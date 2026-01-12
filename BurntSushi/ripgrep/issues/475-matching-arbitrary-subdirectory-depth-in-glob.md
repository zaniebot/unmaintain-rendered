```yaml
number: 475
title: Matching arbitrary subdirectory depth in glob pattern
type: issue
state: closed
author: alexlafroscia
labels: []
assignees: []
created_at: 2017-05-07T20:37:30Z
updated_at: 2017-05-07T23:32:11Z
url: https://github.com/BurntSushi/ripgrep/issues/475
synced_at: 2026-01-12T16:13:22Z
```

# Matching arbitrary subdirectory depth in glob pattern

---

_@alexlafroscia_

I'm running up against an issue with glob patterns, and am hoping you can let me know what I'm doing wrong.  I sometimes end up in a situation where I want to find something just in my app code, not my tests. So I do something like this, trying to search just my app files:

```bash
rg -F 'foobar' -g app/**/*.js
```

thinking that it will search for anything with a `.js` ending that's found in the `app/` directory, regardless of how many subdirectories deep the file might be (this is how other glob patterns I've used before work, such as [`walk-sync`](https://github.com/joliss/node-walk-sync).

However, it seems like this only matches against files that are *exactly* one subdirectory deep; anything directly in `app` isn't found, and anything deeper than one directory below `app` isn't found. How would I properly format the search to find everything?

I've also tried changing it to something like this, to no avail:

```bash
rg -F 'foobar' -tjs -g app/*
rg -F 'foobar' -tjs -g app/**/*
```


---

_Comment by @alexlafroscia on 2017-05-07 20:40_

Aaaand never mind. Path to search can be added as the last parameter, that's exactly what I needed. Whoops!

---

_Closed by @alexlafroscia on 2017-05-07 20:40_

---

_Comment by @BurntSushi on 2017-05-07 21:01_

For what it's worth, I can't actually reproduce the issue you were facing (which seems like it would be a bug):

```
[andrew@Cheetah ripgrep-475] mkdir -p foo/bar/baz/quux
[andrew@Cheetah ripgrep-475] echo test > foo/bar/baz/quux/test.js
[andrew@Cheetah ripgrep-475] rg test -g 'foo/**/*.js'
foo/bar/baz/quux/test.js
1:test
```

---

_Comment by @BurntSushi on 2017-05-07 21:02_

Did you perhaps not put the argument to `-g` in single quotes? If you don't quote it, then your shell will expand it.

---

_Comment by @alexlafroscia on 2017-05-07 22:17_

I'm actually running it from within Vim, so I'm not sure how/if it's being expanded 

---

_Comment by @alexlafroscia on 2017-05-07 22:21_

Maybe it's the running through Vim that's the issue. I'm seeing different results from the command line now. It's weird to me that these commands yield different numbers...

```bash
$ rg this -c -tjs -g app/**/* | wc -l
381

$ rg this -c -tjs -g 'app/**/*' | wc -l
382

$ rg this -c -tjs app/ | wc -l
362
```

---

_Comment by @alexlafroscia on 2017-05-07 22:41_

Huh, I'm having trouble reproducing this now. I'm not sure why it seemed like only one level of subdirectories was being searched, but I wouldn't worry about this. I'll come back and reopen if I come up with a case where I can reliably reproduce the problem.

---

_Comment by @BurntSushi on 2017-05-07 23:32_

All right, thanks! I did just try this in vim (with quotes around the glob) and couldn't reproduce it.

If you do manage to come back around to this, it would be super helpful if you could reproduce it on a public repo that I could clone so that we know we're searching the same stuff. Thanks!

---
