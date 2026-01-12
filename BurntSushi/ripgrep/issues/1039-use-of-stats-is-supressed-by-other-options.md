```yaml
number: 1039
title: Use of --stats is supressed by other options
type: issue
state: closed
author: jamesmudd
labels: []
assignees: []
created_at: 2018-09-05T12:14:52Z
updated_at: 2018-09-05T15:36:26Z
url: https://github.com/BurntSushi/ripgrep/issues/1039
synced_at: 2026-01-12T16:13:22Z
```

# Use of --stats is supressed by other options

---

_@jamesmudd_

#### What version of ripgrep are you using?

ripgrep 0.9.0 (rev 6799dcfc0e)
-SIMD -AVX

#### How did you install ripgrep?

Using the Github release binary https://github.com/BurntSushi/ripgrep/releases/download/0.9.0/ripgrep-0.9.0-x86_64-unknown-linux-musl.tar.gz

#### What operating system are you using ripgrep on?

RHEL6

#### Describe your question, feature request, or bug.

If you use the ``--stats`` option with another option like ``--files-with-matches`` the stats output is suppressed.

#### If this is a bug, what are the steps to reproduce the behavior?
Just `--stats` options works fine
```
$ rg my-searched-string --stats
test
1:my-searched-string

1 matched lines
1 files contained matches
1 files searched
0.021 seconds
```
with another option e.g `--files-with-matches` the stats are not printed
```
$ rg my-searched-string --stats --files-with-matches
test
```

#### If this is a bug, what is the actual behavior?

```
rg my-searched-string --stats --files-with-matches --debug
DEBUG/rg::config/src/config.rs:42: /home/rbl46502/.ripgreprc: arguments loaded from config file: ["--max-columns=120", "--type-add", "mf:MANIFEST.MF", "--type-add", "launch:*.launch", "--type-add", "prop:*.properties", "--type-add", "eclipse:{*.prefs,.classpath,.project,build.properties}", "--type-add", "ant:*.ant", "--hidden", "--smart-case", "--colors", "match:fg:blue", "--colors", "path:fg:red", "--colors", "path:style:bold"]
DEBUG/rg::args/src/args.rs:149: final argv: ["rg", "--max-columns=120", "--type-add", "mf:MANIFEST.MF", "--type-add", "launch:*.launch", "--type-add", "prop:*.properties", "--type-add", "eclipse:{*.prefs,.classpath,.project,build.properties}", "--type-add", "ant:*.ant", "--hidden", "--smart-case", "--colors", "match:fg:blue", "--colors", "path:fg:red", "--colors", "path:style:bold", "my-searched-string", "--stats", "--files-with-matches", "--debug"]
DEBUG/grep::search/grep/src/search.rs:195: original regex HIR pattern:
[Mm][Yy]\-[Ssſ][Ee][Aa][Rr][Cc][Hh][Ee][Dd]\-[Ssſ][Tt][Rr][Ii][Nn][Gg]
DEBUG/grep::search/grep/src/search.rs:197: transformed regex HIR pattern:
[Mm][Yy]\-[Ssſ][Ee][Aa][Rr][Cc][Hh][Ee][Dd]\-[Ssſ][Tt][Rr][Ii][Nn][Gg]
DEBUG/grep::literals/grep/src/literals.rs:87: required literals found: [Cut(MY-SE), Cut(mY-SE), Cut(My-SE), Cut(my-SE), Cut(MY-sE), Cut(mY-sE), Cut(My-sE), Cut(my-sE), Cut(MY-ſE), Cut(mY-ſE), Cut(My-ſE), Cut(my-ſE), Cut(MY-Se), Cut(mY-Se), Cut(My-Se), Cut(my-Se), Cut(MY-se), Cut(mY-se), Cut(My-se), Cut(my-se), Cut(MY-ſe), Cut(mY-ſe), Cut(My-ſe), Cut(my-ſe)]
test

```

#### If this is a bug, what is the expected behavior?

I think the stats output should still be printed as requested


---

_Comment by @BurntSushi on 2018-09-05 12:58_

This is fixed on master (much of the relevant code was rewritten and turned into libraries).

---

_Closed by @BurntSushi on 2018-09-05 12:58_

---

_Comment by @jamesmudd on 2018-09-05 15:36_

Thanks look forward to the next release

---
