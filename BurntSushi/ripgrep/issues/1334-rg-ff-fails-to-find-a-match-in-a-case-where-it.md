```yaml
number: 1334
title: rg -Ff fails to find a match in a case where it should
type: issue
state: closed
author: elindsey
labels:
  - bug
assignees: []
created_at: 2019-07-30T01:15:10Z
updated_at: 2019-08-01T21:14:14Z
url: https://github.com/BurntSushi/ripgrep/issues/1334
synced_at: 2026-01-12T16:13:23Z
```

# rg -Ff fails to find a match in a case where it should

---

_@elindsey_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

brew install ripgrep

#### What operating system are you using ripgrep on?

macos 10.14.6

#### Describe your question, feature request, or bug.

With specific types of input files, I'm seeing rg -Ff return no matches in cases where it should return a match.

#### If this is a bug, what are the steps to reproduce the behavior?

patterns contains the same ip prefix 40 times:
```
$ cat patterns 
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
1.208.0.0/12
```

corpus is a single line containing the prefix from the patterns file:
```
$ cat corpus 
1.208.0.0/12
```

this is expected to produce a match, but does not:
```
$ rg -Ff patterns corpus 
(no matches)
```

treating patterns as non fixed string finds a match:
```
$ rg -f patterns corpus 
1:1.208.0.0/12
```

and strangely, removing a single line from patterns will also find a match:
```
$ head -n 39 patterns > fewerpatterns
$ rg -Ff fewerpatterns corpus 
1:1.208.0.0/12
```

I haven't dug in enough to see if this purely a result of 39 vs 40 lines, or if it's 39 vs 40 lines and the size of the '1.208.0.0/12' string, but I do seem to be crossing some boundary into buggy behavior with this specific test.

[corpus.txt](https://github.com/BurntSushi/ripgrep/files/3444753/corpus.txt)
[fewerpatterns.txt](https://github.com/BurntSushi/ripgrep/files/3444754/fewerpatterns.txt)
[patterns.txt](https://github.com/BurntSushi/ripgrep/files/3444755/patterns.txt)


---

_Label `bug` added by @BurntSushi on 2019-08-01 20:07_

---

_Closed by @BurntSushi on 2019-08-01 21:00_

---

_Comment by @BurntSushi on 2019-08-01 21:00_

Ah thanks for this bug report! Literal optimizations are going to be the death me. I introduced this regression when I added an optimization for the `-F` flag to bypass the regex engine entirely and use Aho-Corasick. There are a number of cases where Aho-Corasick can't be used directly, which I thought I had handled, but one slipped by: ripgrep escapes patterns when the `-F` flag is set before passing them down to the matcher, and those same escaped patterns were being handed to the Aho-Corasick builder directly. So you were _actually_ searching for the literal `1\.208\.0\.0/12`, which is obviously wrong.

The offending line of code is here, which also explains why `40` was a magic number: https://github.com/BurntSushi/ripgrep/blob/bc37c32717301abf26e5aecc942688d2ddd63692/grep-regex/src/matcher.rs#L75

(Specifically, the regex engine, currently, can often search for a small number of literals more quickly than Aho-Corasick, so we were diverting to the regex engine for a smaller number of literals, in which case, using escaped literals is correct.)

This is basically one giant mess because all of this should be pushed down into the regex engine, but there's a lot of refactoring work that needs to be done before that can happen. So I tried to paper over it at a higher level and got bit here.

Anyway, this should be fixed on master. I'm going to see about getting a patch release out soon.

---

_Comment by @elindsey on 2019-08-01 21:14_

Thanks Andrew! I really appreciate both the fix and the explanation - interesting edge case. 😁 

---
