```yaml
number: 1247
title: "11.0.0 regression: Seemingly infinite loop on non-Unicode files"
type: issue
state: closed
author: Deewiant
labels:
  - bug
assignees: []
created_at: 2019-04-16T07:56:31Z
updated_at: 2019-04-16T19:03:23Z
url: https://github.com/BurntSushi/ripgrep/issues/1247
synced_at: 2026-01-12T16:13:23Z
```

# 11.0.0 regression: Seemingly infinite loop on non-Unicode files

---

_@Deewiant_

#### What version of ripgrep are you using?

```
ripgrep 11.0.0 (rev d7f57d9aab)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

And I'm comparing it to:

```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

From the binary releases for `x86_64-unknown-linux-musl`:

* https://github.com/BurntSushi/ripgrep/releases/download/11.0.0/ripgrep-11.0.0-x86_64-unknown-linux-musl.tar.gz
* https://github.com/BurntSushi/ripgrep/releases/download/0.10.0/ripgrep-0.10.0-x86_64-unknown-linux-musl.tar.gz

#### What operating system are you using ripgrep on?

Arch Linux

#### Describe your question, feature request, or bug.

I've run into a crippling performance regression on certain types of queries and non-UTF-8 files between 0.10.0 and 11.0.0, which looks like it might even be an infinite loop.

#### If this is a bug, what are the steps to reproduce the behavior?

A very simple way is to create a file containing only two bytes, "sä" encoded with ISO 8559-1, and search for a pattern with a short prefix that matches the "s" but not the rest, like `'\bs(?:thiswillnotmatch|norwillthis)'`:

```
printf "s\xe4" > test.txt
rg '\bs(?:thiswillnotmatch|norwillthis)' test.txt
```

The `\b` does seem to be required at least in this case.

Another example file that reproduces this is `sherlock.br` in ripgrep's own source code, using the exact same pattern.

#### If this is a bug, what is the actual behavior?

11.0.0 seems to spin forever:

```
$ time rg-11.0 --debug '\bs(?:thiswillnotmatch|norwillthis)' test.txt >/dev/null
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:115: required literal found: "s"
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 3 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes

<it's been 10 minutes and it's still spinning at 100% CPU>
```

#### If this is a bug, what is the expected behavior?

0.10.0 has no problems and gives a result in a few milliseconds:

```
$ time rg-0.10 --debug '\bs(?:thiswillnotmatch|norwillthis)' test.txt >/dev/null
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:110: required literal found: "s"
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 3 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes

0.00user 0.00kernel 0.003elapsed
```

---

_Label `bug` added by @BurntSushi on 2019-04-16 12:33_

---

_Closed by @BurntSushi on 2019-04-16 12:34_

---

_Comment by @BurntSushi on 2019-04-16 12:35_

Thanks for reporting this bug! This was actually a regression introduced in the underlying regex engine (as a result of fixing an unrelated bug). I've published a fix for the regex engine and brought in the updated version on ripgrep master. I'll put out a new point release of ripgrep with this fix soon.

---

_Comment by @BurntSushi on 2019-04-16 18:37_

`ripgrep 11.0.1` is out with this fix in it. Sorry about the regression!

---

_Comment by @Deewiant on 2019-04-16 19:03_

No problem, thanks for the quick response and fix!

---
