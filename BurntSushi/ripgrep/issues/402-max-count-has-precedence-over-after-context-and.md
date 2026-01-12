```yaml
number: 402
title: "--max-count has precedence over --after-context and --context"
type: issue
state: closed
author: chelmertz
labels:
  - bug
  - libripgrep
assignees: []
created_at: 2017-03-10T08:52:56Z
updated_at: 2017-10-08T12:10:36Z
url: https://github.com/BurntSushi/ripgrep/issues/402
synced_at: 2026-01-12T16:13:21Z
```

# --max-count has precedence over --after-context and --context

---

_@chelmertz_

I think `--after-context` (`-A`) and `--context` (`-C`) should have the same precedence as `--before-context` (`-B`), when being combined with `--max-count` (`-m`), but they do not:

```bash
$ cat > rgtest.txt <<EOF
> a
> b
> c
> d
> e
> EOF
$ rg -B2 c rgtest.txt
1-a
2-b
3:c
$ rg -C2 c rgtest.txt
1-a
2-b
3:c
4-d
5-e
$ rg -A2 c rgtest.txt
3:c
4-d
5-e
$ # and now with --max-count:
$ rg -B2 c -m1 rgtest.txt
1-a
2-b
3:c
$ rg -C2 c -m1 rgtest.txt
1-a
2-b
3:c
$ rg -A2 c -m1 rgtest.txt
3:c
```

From my expectations, `-B` works correctly, `-C` works like `-B` which is unexpected, and `-A` has no effect at all.

My use case is to show a colleague one instance of many repeated matches, but also with some context. If `-A` combined with `-m` would yield the same result as with `-B`, then my use case would have been fulfilled.

I am using ripgrep-0.3.2-1.fc24.x86_64, packaged for Fedora through copr (by carlgeorge-ripgrep).

Thanks for a great program! (edit: removed trailing whitespace in code block)

---

_Comment by @BurntSushi on 2017-03-10 11:54_

Thanks for the bug report. This is definitely a bug. But I don't think it has anything to do with *precedence* specifically. For example:

```
[andrew@Cheetah ripgrep-402] cat > rgtest.txt <<EOF
> a
> b
> c
> d
> e
> a
> b
> c
> d
> e
> EOF
[andrew@Cheetah ripgrep-402] rg -C2 c -m1 rgtest.txt
1-a
2-b
3:c
[andrew@Cheetah ripgrep-402] rg -C2 c -m2 rgtest.txt
1-a
2-b
3:c
4-d
5-e
6-a
7-b
8:c
```

Notice that in the last example, the "after" context for the first match is printed correctly. Only the last match loses it.

This is probably just a bug in the search code where it neglects to print the context after the last match if it decides to quit early. I'll note that `grep` handles this correctly:

```
[andrew@Cheetah ripgrep-402] grep -C2 c -m2 rgtest.txt
a
b
c
d
e
a
b
c
d
e
```

This _could_ be a little tricky to fix. Not sure.

---

_Label `bug` added by @BurntSushi on 2017-03-10 11:54_

---

_Label `libripgrep` added by @BurntSushi on 2017-03-10 11:54_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-03-10 11:54_

---

_Closed by @BurntSushi on 2017-10-08 12:10_

---
