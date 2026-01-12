```yaml
number: 1203
title: ripgrep fails to match pattern including digit character class
type: issue
state: closed
author: ravron
labels:
  - bug
assignees: []
created_at: 2019-02-27T20:30:11Z
updated_at: 2019-02-27T23:18:19Z
url: https://github.com/BurntSushi/ripgrep/issues/1203
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep fails to match pattern including digit character class

---

_@ravron_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

```
$ brew tap burntsushi/ripgrep https://github.com/BurntSushi/ripgrep.git
$ brew install ripgrep-bin
```

#### What operating system are you using ripgrep on?

macOS 10.14.3 (18D109)

#### Describe your question, feature request, or bug.

`rg` appears to fail to find a certain pattern in a one-line file that definitely contains that pattern.

I must be missing something — this seems very unlikely to be a legitimate bug — but I can't figure out what.

#### If this is a bug, what are the steps to reproduce the behavior?

1. `echo 153.230000 >| test.txt`
2. `rg '\d\d\d00' test.txt`. This successfully finds a match of `23000`.
3. `rg '\d\d\d000' test.txt`. This _fails_ to find any match, when it should match `230000`

Note that `grep '\d\d\d000' test.txt` correctly matches `230000`. (`grep --version` `grep (BSD grep) 2.5.1-FreeBSD`)

#### If this is a bug, what is the actual behavior?

```
$ echo 153.230000 >| test.txt
$ rg --debug '\d\d\d000' test.txt
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:110: required literal found: "000"
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "**/.*.s[a-w][a-z]", re: "(?-u)^(?:/?|.*/)\\..*\\.s[a-w][a-z]$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('.'), ZeroOrMore, Literal('.'), Literal('s'), Class { negated: false, ranges: [('a', 'w')] }, Class { negated: false, ranges: [('a', 'z')] }]) }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 3 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
$
```

#### If this is a bug, what is the expected behavior?

`rg '\d\d\d000' test.txt` should identify the single match in the file, as `grep` does. Specifically:

```
$ rg '\d\d\d000' test.txt
1:153.230000
```

#### Other

Note that changing the corpus in seemingly irrelevant ways can cause the bug to change or disappear. For example, the `\d\d\d000` pattern matches if three `0` characters are prepended to the contents of the file (that is, the file contains `000153.230000`). 



---

_Label `bug` added by @BurntSushi on 2019-02-27 21:03_

---

_Closed by @BurntSushi on 2019-02-27 22:43_

---

_Comment by @BurntSushi on 2019-02-27 22:46_

Thanks so much for reporting this! It is indeed a bug in the regex engine. You can [this](https://github.com/rust-lang/regex/commit/661bf53d5b2b6dde25549aaad601ad8c59b37bfd) and [this](https://github.com/rust-lang/regex/commit/edf45e6f5fa54705298ba14f3216cfb5277c0908) for the specific changes to the regex engine to fix this. The short story is that you were tripping in a reverse suffix literal optimization that wasn't quite correct. The reason why it seemed sensitive to different regexes and inputs is because it is! :-) The reverse suffix literal optimization only runs in very specific circumstances related to the size and structure of the regex, and this particular bug is only tripped when a suffix literal (such as `000`) can result in potentially overlapping matches. In this case, both your pattern and your haystack did this.

This should now be fixed on master, since I've bumped ripgrep's regex dependency to `1.1.2`.

---

_Comment by @ravron on 2019-02-27 23:18_

No kidding. Thanks for the explanation and the fixes!

---
