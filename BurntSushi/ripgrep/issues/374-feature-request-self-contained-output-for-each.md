```yaml
number: 374
title: "feature request: self-contained output for each match, like ag --vimgrep"
type: issue
state: closed
author: Dubhead
labels:
  - question
assignees: []
created_at: 2017-02-21T19:55:09Z
updated_at: 2017-02-21T22:23:44Z
url: https://github.com/BurntSushi/ripgrep/issues/374
synced_at: 2026-01-12T16:13:21Z
```

# feature request: self-contained output for each match, like ag --vimgrep

---

_@Dubhead_

```
$ ag --vimgrep search_stream
src/main.rs:53:5:mod search_stream;
src/worker.rs:13:5:use search_stream::{InputBuffer, Searcher};
src/search_buffer.rs:6:62:Note that this module doesn't quite support everything that `search_stream` does.
src/search_buffer.rs:16:5:use search_stream::{IterLines, Options, count_lines, is_binary};
src/search_stream.rs:2:6:The `search_stream` module is responsible for searching a single file and
$
```
Each line contains file name, line number, column number, and matched text.  This format is useful for Vim's QuickFix.  My Vim configuration has this:

```
set grepprg=/usr/bin/ag\ --vimgrep
set grepformat=%f:%l:%c:%m
```

When I do `:grep` in Vim, the cursor automatically jumps to the first match.  I do `:cnext` for other matches.

It would be nice if ripgrep can be integrated with Vim in a similar way.

---

_Comment by @BurntSushi on 2017-02-21 22:14_

```
$ rg -h | rg vimgrep
79:        --vimgrep                          Show results in vim compatible format.
```

---

_Closed by @BurntSushi on 2017-02-21 22:14_

---

_Label `question` added by @BurntSushi on 2017-02-21 22:14_

---

_Comment by @BurntSushi on 2017-02-21 22:15_

```
$ rg --vimgrep --no-heading search_stream
src/worker.rs:13:5:use search_stream::{InputBuffer, Searcher};
src/search_buffer.rs:6:62:Note that this module doesn't quite support everything that `search_stream` does.
src/search_buffer.rs:16:5:use search_stream::{IterLines, Options, count_lines, is_binary};
src/search_stream.rs:2:6:The `search_stream` module is responsible for searching a single file and
src/main.rs:49:5:mod search_stream;
```

---

_Comment by @Dubhead on 2017-02-21 22:23_

I have overlooked the `--no-heading` option.  Thank you.

---
