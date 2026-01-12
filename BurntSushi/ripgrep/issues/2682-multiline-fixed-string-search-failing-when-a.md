```yaml
number: 2682
title: Multiline fixed string search failing when a newline character is followed by the ^ character
type: issue
state: closed
author: learnbyexample
labels: []
assignees: []
created_at: 2023-12-11T08:30:56Z
updated_at: 2023-12-15T13:09:09Z
url: https://github.com/BurntSushi/ripgrep/issues/2682
synced_at: 2026-01-12T16:13:24Z
```

# Multiline fixed string search failing when a newline character is followed by the ^ character

---

_@learnbyexample_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.0.3

### How did you install ripgrep?

Using ripgrep_14.0.3-1_amd64.deb

### What operating system are you using ripgrep on?

Ubuntu 20.04

### Describe your bug.

Multiline fixed string search doesn't work when a newline character is followed by the `^` character.

### What are the steps to reproduce the behavior?

Any multiline fixed string search for a literal newline character followed by a `^` character.

### What is the actual behavior?

```bash
$ s='apple fig\n^mango banana\n123 456\ndragon$\nunicorn\n'

# works as expected
$ printf '%b' "$s" | rg -UF 'banana
123'
^mango banana
123 456

# failing case, no output when newline is followed by ^
$ printf '%b' "$s" | rg -UF 'fig
^mango'
# works if regex is used instead of fixed string
$ printf '%b' "$s" | rg -U 'fig\n\^mango'
apple fig
^mango banana
# interestingly, this also works - not sure what's the difference compared to the failing case
$ printf '%b' "$s" | rg -UF $'fig\n^mango'
apple fig
^mango banana
# no issue if the first line starts with ^
$ printf '%b' "$s" | rg -UF '^mango banana
123'
^mango banana
123 456

# no issue if there's a $ at the end of a line
$ printf '%b' "$s" | rg -UF '456
dragon$'
123 456
dragon$
```

### What is the expected behavior?

Fixed string search should work irrespective of the character being searched.

---

_Comment by @BurntSushi on 2023-12-11 13:41_

Interestingly, I cannot reproduce this:

```
$ s='apple fig\n^mango banana\n123 456\ndragon$\nunicorn\n'
$ printf '%b' "$s" | rg -UF 'banana
123'
^mango banana
123 456
$ printf '%b' "$s" | rg -UF 'fig
^mango'
apple fig
^mango banana
```

Can you re-run with `--trace`? That might show some information.

I can't make heads or tails of this. The search shouldn't care about `^`.

---

_Comment by @learnbyexample on 2023-12-11 14:17_

Oh. I did wonder if there's something wrong/different on my machine. Here's the `--trace` output:

```bash
$ printf '%b' "$s" | rg -UF --trace 'fig
^mango'
DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1112: heuristic chose to search stdin
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: abcd
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
DEBUG|grep_regex::config|crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
TRACE|grep_regex::matcher|crates/regex/src/matcher.rs:66: final regex: "(?:fig\n!!:s\\^mango)"
TRACE|grep_regex::literal|crates/regex/src/literal.rs:59: skipping inner literal extraction, no line terminator is set
DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|rg::search|crates/core/search.rs:254: <stdin>: binary detection: BinaryDetection(Convert(0))
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:728: generic reader: reading everything to heap for multiline
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:732: generic reader: searching via multiline strategy
```

Final regex `(?:fig\n!!:s\\^mango)` seems very odd, compared to `(?:banana\n123)` and `(?:fig\n\\^mango)` (when I use `$'fig\n^mango'`).

---

_Comment by @BurntSushi on 2023-12-11 14:31_

Yeah, odd indeed. Here's mine:

```
$ printf '%b' "$s" | rg --trace -UF 'fig
^mango'
DEBUG|rg::flags::config|crates/core/flags/config.rs:41: /home/andrew/.ripgreprc: arguments loaded from config file: ["--max-columns-preview", "--colors=match:bg:0xff,0x7f,0x00", "--colors=match:fg:white", "--colors=line:none", "--colors=line:fg:magenta", "--colors=path:fg:green", "--type-add=got:*_test.go"]
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1112: heuristic chose to search stdin
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: duff
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
DEBUG|grep_regex::config|crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
TRACE|grep_regex::matcher|crates/regex/src/matcher.rs:66: final regex: "(?:fig\n\\^mango)"
TRACE|grep_regex::literal|crates/regex/src/literal.rs:59: skipping inner literal extraction, no line terminator is set
DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|rg::search|crates/core/search.rs:254: <stdin>: binary detection: BinaryDetection(Convert(0))
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:728: generic reader: reading everything to heap for multiline
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:732: generic reader: searching via multiline strategy
apple fig
^mango banana
```

Where the regex is `"(?:fig\n\\^mango)"` is what I'd expect.

You seem to be having some kind of extra `!!:s` being inserted between the `\n` and the `\^`. I dunno where that is coming from. Can you try a different shell?

Okay, that made _me_ try a different shell. I use zsh by default. So I dropped into bash:

```
[andrew@duff ripgrep] printf '%b' "$s" | rg --trace -UF 'fig
^mango'
DEBUG|rg::flags::config|crates/core/flags/config.rs:41: /home/andrew/.ripgreprc: arguments loaded from config file: ["--max-columns-preview", "--colors=match:bg:0xff,0x7f,0x00", "--colors=match:fg:white", "--colors=line:none", "--colors=line:fg:magenta", "--colors=path:fg:green", "--type-add=got:*_test.go"]
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1112: heuristic chose to search stdin
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: duff
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
DEBUG|grep_regex::config|crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
TRACE|grep_regex::matcher|crates/regex/src/matcher.rs:66: final regex: "(?:fig\n!!:s\\^mango)"
TRACE|grep_regex::literal|crates/regex/src/literal.rs:59: skipping inner literal extraction, no line terminator is set
DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|rg::search|crates/core/search.rs:254: <stdin>: binary detection: BinaryDetection(Convert(0))
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:728: generic reader: reading everything to heap for multiline
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:732: generic reader: searching via multiline strategy
```

This sadly therefore looks like something bash is doing. I haven't the faintest clue what it is. And I'm surprised by it, given that single quotes are being used here.

---

_Comment by @BurntSushi on 2023-12-11 14:32_

```
$ echo 'fig
^mango'
fig
!!:s^mango
```

lolwut

---

_Comment by @learnbyexample on 2023-12-11 14:45_

Oh! Didn't expect that! I'll try to check the `bash` bug list tomorrow. It feels like a regression, since this was working for me 3 years ago.

---

_Comment by @BurntSushi on 2023-12-11 15:21_

I posted [about this on twitter](https://twitter.com/burntsushi5/status/1734223796914028647), and it looks like some kind interaction substitution based on history. But the fact that it's happening inside a single quoted string suggests this might be a bash bug.

---

_Comment by @learnbyexample on 2023-12-15 03:48_

Discussion in the bug-bash group: https://lists.gnu.org/archive/html/bug-bash/2023-12/msg00064.html

---

_Closed by @learnbyexample on 2023-12-15 03:48_

---

_Comment by @BurntSushi on 2023-12-15 09:04_

Thanks for the follow-up! I read through that thread and I can't even tell whether they consider the behavior a bug or not. It sounds like they are just going to fix the docs?

---

_Comment by @learnbyexample on 2023-12-15 09:46_

I think they are going to fix it for single quotes (along with documentation update):

>The quick substitution feature didn't pay attention to the quoting state,
>however, and just translated the line into a standard history expansion,
>which was then skipped. The fix is to have it inhibit quick substitution
>the same way as other history expansions.

---

_Comment by @BurntSushi on 2023-12-15 13:09_

Yeah I saw that. But the other part of their message was "This is a standard form of history expansion, described in the man page." That confused me hah. Either way, quite interesting!

---
