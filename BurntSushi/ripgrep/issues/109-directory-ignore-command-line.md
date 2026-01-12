```yaml
number: 109
title: Directory ignore command line
type: issue
state: closed
author: gsquire
labels:
  - enhancement
assignees: []
created_at: 2016-09-26T21:37:17Z
updated_at: 2016-09-27T23:46:29Z
url: https://github.com/BurntSushi/ripgrep/issues/109
synced_at: 2026-01-12T18:23:11Z
```

# Directory ignore command line

---

_@gsquire_

Do you think it would be valuable to add an option to skip traversing directories? I know I can use an `.rgignore` file but it's not convenient to copy it from repository to repository.

If so, I think I could try and implement it as well.


---

_Comment by @lzybkr on 2016-09-26 21:50_

I was just looking for an option to not recurse, so yeah, that'd be useful.


---

_Comment by @BurntSushi on 2016-09-26 21:56_

I think `rg -g '!*/' PAT` will do it.


---

_Comment by @BurntSushi on 2016-09-26 21:57_

That's not to say we don't want, say, a `--depth` flag or something that sets a limit on the recursion depth. I think I'd be OK with that. (Certainly, `find` has it.) Maybe use the same flag name as `find`? `--maxdepth`. A depth of `0` could mean "only search current directory."


---

_Comment by @gsquire on 2016-09-26 22:00_

Ah I missed the `-g` flag. I suppose I need to read the usage more carefully. I imagine max depth would be easy to add considering you use your `walkdir` crate too :)


---

_Comment by @BurntSushi on 2016-09-26 22:26_

Yes indeedy! Let me know if you'd like to work on it and I won't touch it. :-)

(I think `maxdepth` should be relatively straight-forward. `mindepth` would not be. Thankfully, there's less of a use case for that I think.)


---

_Comment by @gsquire on 2016-09-26 22:28_

I would be happy to try it tonight and I'll keep you updated in this thread. Thanks!


---

_Assigned to @gsquire by @BurntSushi on 2016-09-26 22:29_

---

_Comment by @BurntSushi on 2016-09-26 22:29_

Awesome, thank you. :-)


---

_Label `enhancement` added by @BurntSushi on 2016-09-26 22:51_

---

_Closed by @BurntSushi on 2016-09-27 23:46_

---
