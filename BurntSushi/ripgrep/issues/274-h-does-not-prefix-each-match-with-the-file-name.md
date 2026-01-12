```yaml
number: 274
title: "-H does not \"prefix each match with the file name that contains it\""
type: issue
state: closed
author: bwo
labels: []
assignees: []
created_at: 2016-12-07T23:11:54Z
updated_at: 2016-12-12T11:57:52Z
url: https://github.com/BurntSushi/ripgrep/issues/274
synced_at: 2026-01-12T16:13:21Z
```

# -H does not "prefix each match with the file name that contains it"

---

_@bwo_

instead, it prefixes each *set* of matches from a given file with the name of the file containing the matches, on its own line:

```
Ben-Wolfson-Tower ~/Downloads/ripgrep-0.3.2-i686-unknown-linux-musl 03:09:13 $ rg -H foo
README.md
76:* `ripgrep` can search specific types of files. For example, `rg -tpy foo`
77:  limits your search to Python files and `rg -Tjs foo` excludes Javascript
205:$ rg foobar
219:$ rg -uu foobar  # similar to `grep -r`
220:$ rg -uuu foobar  # similar to `grep -a -r`
254:$ rg foo -g 'README.*'
262:$ rg foo -g '!*.min.js'
268:$ rg -thtml -tcss foobar
274:$ rg -Tjs foobar
282:$ rg --type-add 'foo:*.{foo,foobar}' -tfoo bar
285:The type `foo` will now match any file ending with the `.foo` or `.foobar`

rg.1
165:\[aq]line:bg:yellow\[aq] foo.
324:Capture group indices (e.g., $5) and names (e.g., $foo) are supported in
378:\f[C]rg\ \-\-type\-add\ \[aq]foo:*.foo\[aq]\ \-tfoo\ PATTERN\f[]
```

cf. `grep` with the `-H` option:

```
Ben-Wolfson-Tower ~/Downloads/ripgrep-0.3.2-i686-unknown-linux-musl 03:11:02 $ grep -nH foo *
grep: complete: Is a directory
README.md:76:* `ripgrep` can search specific types of files. For example, `rg -tpy foo`
README.md:77:  limits your search to Python files and `rg -Tjs foo` excludes Javascript
README.md:205:$ rg foobar
README.md:219:$ rg -uu foobar  # similar to `grep -r`
README.md:220:$ rg -uuu foobar  # similar to `grep -a -r`
README.md:254:$ rg foo -g 'README.*'
README.md:262:$ rg foo -g '!*.min.js'
README.md:268:$ rg -thtml -tcss foobar
README.md:274:$ rg -Tjs foobar
README.md:282:$ rg --type-add 'foo:*.{foo,foobar}' -tfoo bar
README.md:285:The type `foo` will now match any file ending with the `.foo` or `.foobar`
Binary file rg matches
rg.1:165:\[aq]line:bg:yellow\[aq] foo.
rg.1:324:Capture group indices (e.g., $5) and names (e.g., $foo) are supported in
rg.1:378:\f[C]rg\ \-\-type\-add\ \[aq]foo:*.foo\[aq]\ \-tfoo\ PATTERN\f[]
```

This makes `rg` unsuitable for use with e.g. Emacs's `rgrep`, since one cannot jump to matches.

---

_Comment by @BurntSushi on 2016-12-07 23:34_

@bwo ripgrep has many options that may be useful to you. For example, `--no-heading` or `--vimgrep`. Could you please try them and let me know whether they satisfy your use case or not?

---

_Comment by @BurntSushi on 2016-12-07 23:35_

You may also find the output of `rg foo README.md | cat` illuminating.

---

_Comment by @BurntSushi on 2016-12-12 11:57_

It's my belief that the OP's problem is resolved by existing options. If that's wrong, feel free to re-open!

---

_Closed by @BurntSushi on 2016-12-12 11:57_

---
