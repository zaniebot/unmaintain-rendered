```yaml
number: 1064
title: "Obvious match missing from results when using group: a(.*c) does not match abc"
type: issue
state: closed
author: phiresky
labels:
  - bug
assignees: []
created_at: 2018-09-24T18:41:37Z
updated_at: 2018-09-25T20:56:06Z
url: https://github.com/BurntSushi/ripgrep/issues/1064
synced_at: 2026-01-12T16:13:22Z
```

# Obvious match missing from results when using group: a(.*c) does not match abc

---

_@phiresky_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)


#### Bug

Example:

`echo abc | rg --debug 'a(.*c)'`

```
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:110: required literal found: "ac"
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

doesn't find any results. The following all work:

`echo ac | rg 'a(.*c)'`

`echo abc | rg 'a.*c'`

`echo abc | ag 'a(.*c)'`

`echo abc | grep 'a\(.*c\)'`

---

_Comment by @lespea on 2018-09-24 20:11_

Interestingly enough this also fails: `echo abc | rg --debug 'a(?:.*c)'` but this works: `echo abc | rg --debug 'a(.*)c'`

---

_Comment by @BurntSushi on 2018-09-24 20:17_

Ug. This looks like a bug in ripgrep's inner literal extractor. It thinks `ac` is a literal that must appear in every match, but this is obviously wrong. The regex engine prefix/suffix detection is correct:

```
[andrew@Cheetah ~]$ regex-debug prefixes 'a(.*c)'
Cut(a)
[andrew@Cheetah ~]$ regex-debug suffixes 'a(.*c)'
Cut(c)
```

I diagnosed this from the `--debug` output. Cutting out the irrelevant stuff, you can see the problem here:

```
[andrew@Cheetah ~]$ echo abc | rg 'a(.*c)' --debug
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:110: required literal found: "ac"
[andrew@Cheetah ~]$ echo abc | rg 'a.*c' --debug
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:110: required literal found: "a"
```

I would have thought this was a regression since I touched this code as part of libripgrep work, but 0.9.0 exhibits the same bug.

---

_Label `bug` added by @BurntSushi on 2018-09-24 20:17_

---

_Comment by @phiresky on 2018-09-24 20:19_

checking out ripgrep 0.0.1 (with `regex = "0.1.75"`) has the same bug

---

_Closed by @BurntSushi on 2018-09-25 20:56_

---
