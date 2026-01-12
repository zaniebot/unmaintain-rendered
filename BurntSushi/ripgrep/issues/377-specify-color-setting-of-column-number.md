```yaml
number: 377
title: Specify color setting of column number
type: issue
state: closed
author: ShikChen
labels:
  - question
assignees: []
created_at: 2017-02-23T10:16:01Z
updated_at: 2017-04-09T13:09:06Z
url: https://github.com/BurntSushi/ripgrep/issues/377
synced_at: 2026-01-12T16:13:21Z
```

# Specify color setting of column number

---

_@ShikChen_

Currently (0.4.0) the `--colors` option only supports types path, line and match. There is no way to specify the color of column number now. So when using with `--column`, the column part is uncolored and has the same style of matching line, which is a little unpleasant.

---

_Comment by @BurntSushi on 2017-02-23 11:43_

@ShikChen Out of curiosity, what do you use the column information for?

---

_Comment by @ShikChen on 2017-02-23 12:01_

I use it for vim, which needs the column information to jump into exact location. The above statement is also true for option `--vimgrep`. More precisely, I use it with [fzf.vim](https://github.com/junegunn/fzf.vim).

---

_Comment by @BurntSushi on 2017-02-23 12:14_

@ShikChen Sorry, I wasn't clear. Color information is *for humans*. What do *you* use the column information for? I understand that automated tools may read it to do useful things, but automated tools don't need colors.

---

_Comment by @ShikChen on 2017-02-24 10:38_

But I will see the column information in the UI of fzf.vim, if I want to jump with exact location. When the matching line starts without indent (white spaces), my eyes cannot easily parse boundary between the column number and matching line, especially when that line is starting with numbers.

I agree that most of time we don't need line/column numbers. But it's still possible that someone is manipulating tabular data like csv, and need line/column information to distinguish between multiple matches.

I can understand if you decide not to add color to column number. The default search tool in fzf is [Ag](https://github.com/ggreer/the_silver_searcher), which doesn't support it either. Nobody complains about that so maybe I am too sensitive.

Inspired from your comment, now I have some vimscripts that hiding the line/column part in fzf.

---

_Label `question` added by @BurntSushi on 2017-02-24 11:54_

---

_Comment by @BurntSushi on 2017-02-24 11:55_

FWIW, I do actually think line numbers are useful to be able to read because it helps orient our minds to where something is in a file (and possibly relative to other things as well). But column numbers seem less useful since lines are generally pretty short and the matches themselves are highlighted with color.

I'll leave this open for now and mull on it some more.

---

_Comment by @ShikChen on 2017-03-02 16:48_

I found another scenario that I want the column number recently, although it's a little complex and weird. I have mapped a hotkey in vim to search the current word under the cursor in ripgrep with fzf, and I can interactively filter the results in fzf.  I sometimes use the column number as filter. For example, when tracing the code of Bash, I use the column number `:1:` to select the function definition. (I know that I can use `^` to match the prefix in normal ripgrep/fzf usage, but it's just harder to use in my scenario.)

---

_Comment by @kpp on 2017-03-31 19:53_

The case of reading columns is `-o` flag.

![Image of example](http://oi65.tinypic.com/9gzeqq.jpg)

There is only one possibility to differ on bar from another bar if they are both on the same line.

---

_Comment by @BurntSushi on 2017-03-31 19:55_

@kpp That is interesting, and I think I buy that use case. However, if `--only-matching` only shows the matched portion of the line and both the match and the line number are colored, the column number will actually be distinct even though it's not specifically colorized.

---

_Comment by @kpp on 2017-03-31 19:56_

How about `--colors=match:none`?

---

_Comment by @BurntSushi on 2017-03-31 20:00_

Seems contrived to me.

---

_Comment by @kpp on 2017-03-31 20:05_

How about `echo -e "foo\nfoo\n1:bar" | rg foo -n -A1 --column`?

---

_Comment by @kpp on 2017-03-31 20:17_

@BurntSushi  An options is to reset column color by default, but let users to control it if they want to.

---

_Closed by @BurntSushi on 2017-04-09 13:09_

---
