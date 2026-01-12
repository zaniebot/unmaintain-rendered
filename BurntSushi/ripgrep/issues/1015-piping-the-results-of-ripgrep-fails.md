```yaml
number: 1015
title: Piping the results of ripgrep fails
type: issue
state: closed
author: Vagelis-Prokopiou
labels:
  - invalid
assignees: []
created_at: 2018-08-16T08:49:10Z
updated_at: 2018-08-16T11:22:04Z
url: https://github.com/BurntSushi/ripgrep/issues/1015
synced_at: 2026-01-12T16:13:22Z
```

# Piping the results of ripgrep fails

---

_@Vagelis-Prokopiou_

#### What version of ripgrep are you using?

ripgrep 0.9.0 (rev 6799dcfc0e)

#### How did you install ripgrep?

I placed the rg.exe from ripgrep-0.9.0-x86_64-pc-windows-gnu to C:\\User\bin

#### What operating system are you using ripgrep on?

Windows 7 (through Git Bash).

#### Describe your question, feature request, or bug.

Piping the results of ripgrep fails. Possibly related to #890.

**Grep behavior:**
Command: grep -lr "):'" .
Result: path/to/matching/file

Command: grep -lr "):'" . | while read line; do echo "$line"; done;
Result: path/to/matching/file

**Ripgrep behavior:**
Command: rg -Fl "):'" .
Result: path\to\matching\file (which is right since we are on Windows)

Command: rg -Fl "):'" . | while read line; do echo "$line"; done;
Result: pathtomatchingfile (all the path separators are gone...)

The debug flad provides the following:
DEBUG/grep::search/grep\src\search.rs:195: original regex HIR pattern:
\):'
DEBUG/grep::search/grep\src\search.rs:197: transformed regex HIR pattern:
\):'
DEBUG/grep::literals/grep\src\literals.rs:36: literal prefixes detected: Literals { lits: [Complete():')], limit_size: 250, limit_class: 10 }
DEBUG/globset/globset\src\lib.rs:424: glob converted to regex: Glob { glob: "Libs/TempTestCase*", re: "(?-u)^Libs/TempTestCase[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('L'), Literal('i'), Literal('b'), Literal('s'), Literal('/'), Literal('T'), Literal('e'), Literal('m'), Literal('p'), Literal('T'), Literal('e'), Literal('s'), Literal('t'), Literal('C'), Literal('a'), Literal('s'), Literal('e'), ZeroOrMore]) }
DEBUG/globset/globset\src\lib.rs:424: glob converted to regex: Glob { glob: "**/Temp*", re: "(?-u)^(?:/?|.*/)Temp.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('T'), Literal('e'), Literal('m'), Literal('p'), ZeroOrMore]) }
DEBUG/globset/globset\src\lib.rs:429: built glob set; 1 literals, 4 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 2 regexes



#### If this is a bug, what are the steps to reproduce the behavior?

Reproduce be running the following search:
`rg -Fl "searchTerm" . | while read line; do echo "$line"; done;`

What do you think ripgrep should have done?
I suppose that it should at least return a result like `path\to\matching\file`.

I don't know if this is a ripgrep bug per se. I just decided to create this issue to provide more info on the piping subject under Windows.

Thanx for this awesome tool.


---

_Comment by @BurntSushi on 2018-08-16 10:56_

Yeah this isn't a problem with ripgrep. Others are welcome to do a deeper dive here, but [according to the Git for Windows docs](https://github.com/git-for-windows/git/wiki), it works similarly to cygwin in that it provides a POSIX layer on top of Windows since `git` isn't a portable application. ripgrep, on the other hand, *is* portable and does not require any sort of POSIX layer. It is a **native Windows application**. That means you're trying to use a native Windows application in a POSIX environment, where that POSIX environment expects `/` as a separator. Since ripgrep is a native Windows application, it uses `\` as a separator. `\` is not a path separator recognized by POSIX, and is instead often used as a way to delineate escapes in strings, which is likely what's happening here. cygwin has facilities for doing path translation such that it understands `\`, but it looks like Git for Windows does not. You, the user, must deal with that.

This is why ripgrep has a `--path-separator` flag. Set it to `/` in your [ripgrep config file](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file) and be happy that ripgrep will now play nicely with your POSIX emulation layer.

(This is exactly the suggested work around as mentioned [here](https://github.com/BurntSushi/ripgrep/issues/890#issuecomment-406878797) in #890.)

---

_Closed by @BurntSushi on 2018-08-16 10:56_

---

_Label `invalid` added by @BurntSushi on 2018-08-16 10:57_

---

_Comment by @Vagelis-Prokopiou on 2018-08-16 11:22_

Indeed, the following command works as expected:
`rg --path-separator //  -Fl "searchTerm" . | while read line; do echo "$line"; done;`
Result: `path/to/matching/file`

PS: Thanx for the feedback Andrew!
Damn Windows...

---
