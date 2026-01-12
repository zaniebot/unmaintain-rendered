```yaml
number: 725
title: glob ignored for items in path
type: issue
state: closed
author: tekumara
labels:
  - question
assignees: []
created_at: 2018-01-01T00:36:53Z
updated_at: 2018-01-11T13:05:53Z
url: https://github.com/BurntSushi/ripgrep/issues/725
synced_at: 2026-01-12T16:13:22Z
```

# glob ignored for items in path

---

_@tekumara_

I found the following surprising:
```
$ echo bar > bar
$ rg -g 'foo' --files *
bar
$ rg -g 'foo' --files bar
bar
```

I was expecting the above rg commands to return nothing.

---

_Comment by @BurntSushi on 2018-01-01 00:49_

Interesting, I guess `grep` has the behavior you would expect:

```
[andrew@Cheetah ripgrep-725] grep -r test
foo:test
bar:test
[andrew@Cheetah ripgrep-725] grep -r test --include foo
foo:test
[andrew@Cheetah ripgrep-725] grep -r test --include foo *
foo:test
[andrew@Cheetah ripgrep-725] grep test --include foo *
foo:test
[andrew@Cheetah ripgrep-725] grep test --include foo bar
[andrew@Cheetah ripgrep-725]
```

I think my thinking here was that if you specified a file on the command line, then that meant you explicitly wanted to search it, so nothing should override it. I suppose that is perhaps not exactly true if you're globbing or getting your file list from some other process.

At the very least, grep's behavior is a strong vote in favor of respecting explicit globs even on explicitly given file paths.

---

_Comment by @okdana on 2018-01-01 01:00_

AFAIK `ag` behaves the same way as `rg` here, and i think it makes sense, personally. Glob rules in particular tend to be used for matching against multiple files, so it seems reasonable that an explicit file path (which can only ever match *one* file, and which the user has to go out of their way to provide) has precedence. It also matches the way ignore rules can be overridden, which makes sense to me too because command-line globs and ignore files are closely related functionality

I can see the other side of it too but that's my intuition anyway

---

_Comment by @BurntSushi on 2018-01-01 01:09_

@okdana In the case of `rg -g foo bar` yeah I see how that makes sense. But doesn't `rg -g foo *` kind of turn you around in the other direction? And there's also `find ./ ... | xargs rg -g foo` as well.

But yeah, I don't know what to do here.

---

_Comment by @okdana on 2018-01-01 01:22_

>But doesn't rg -g foo * kind of turn you around in the other direction?

It might, if `rg` was handling the `*` — but `rg` doesn't know anything about that, it's a shell feature. By the time `rg` gets it it could have expanded to a hundred files or just one, and it has no way of telling either way.

Passing `*` to a recursive search tool in the first place usually stems from a misunderstanding of how either the shell or the tool works, IMO. There are a few cases where it's useful, but 95% of the time it's redundant or even counter-productive.

>And there's also find ./ ... | xargs rg -g foo as well.

I never thought about that. But i guess if you're already involving `find` there's no reason not to use its own features (which are usually more powerful than `rg`'s anyway, tbh) to include/exclude file paths.

🤷‍♀️

---

_Comment by @BurntSushi on 2018-01-01 01:46_

Yeah, I guess I'm inclined to say this: unless someone can come up with a compelling reason to switch today's behavior, then I'd prefer to stay with what we have today. I do somewhat consider "that's what grep does" to be a strong vote in favor of switching, but I'd like a little more than that.

---

_Label `question` added by @BurntSushi on 2018-01-01 01:46_

---

_Comment by @tekumara on 2018-01-01 22:13_

It might be nice to have the behaviour documented for those coming from `grep`.

---

_Closed by @BurntSushi on 2018-01-11 13:05_

---
