```yaml
number: 971
title: Is it possible to merge the lines in rg result?
type: issue
state: closed
author: wsdjeg
labels:
  - question
assignees: []
created_at: 2018-07-05T04:37:04Z
updated_at: 2018-07-06T12:58:49Z
url: https://github.com/BurntSushi/ripgrep/issues/971
synced_at: 2026-01-12T16:13:22Z
```

# Is it possible to merge the lines in rg result?

---

_@wsdjeg_

#### What version of ripgrep are you using?

```
ripgrep 0.8.1
-SIMD -AVX
```

#### How did you install ripgrep?

If you installed ripgrep with snap and are getting strange file permission or
file not found errors, then please do not file a bug. Instead, use one of the
Github binary releases.

Just using default arch package manager `pacman`.

#### What operating system are you using ripgrep on?

`uname -a`

```
Linux archlinux 4.17.3-1-ARCH #1 SMP PREEMPT Tue Jun 26 04:42:36 UTC 2018 x86_64 GNU/Linux
```

#### Describe your question, feature request, or bug.

I think it is juat a question, how could I merge the lines in the results? in current version, when I search text, sometimes I see two or more lines with same file path and line number ( they have different column), I hope rg can merge the results with same file path and line number.

**Why: **

I try to improve the flygrep.vim, which is a vim plugin, in the plugin I use a job to start rg, the stdout will trigger callback function. but vim script is too slow to parse and merge the lines, I hope `rg` can do it instead.

for more info see https://github.com/SpaceVim/SpaceVim/pull/1898


---

_Comment by @BurntSushi on 2018-07-05 10:42_

Sorry, but this question/bug report isn't actionable. I see no reproduction steps. I see no report of expected output or actual output. Please provide a ripgrep command, along with a corpus, that demonstrates what you're talking about.

If I had to guess, I'd say it sounds like you're **specifically** using a flag that causes ripgrep to emit a new line for every match. Such flags include `--only-matching` and `--vimgrep`. If you don't want that behavior, then don't use those flags.

---

_Label `question` added by @BurntSushi on 2018-07-05 10:43_

---

_Comment by @okdana on 2018-07-05 17:21_

@wsdjeg if you're trying to get the output format of `--vimgrep` but without the separate-line-per-match behaviour, i think you want this:

```
rg --color=never --no-heading --with-filename --line-number --column
```

e.g.:

```
% print -rl 'foobar foobaz' foobar baz |
  \rg --vimgrep foo
<stdin>:1:1:foobar foobaz
<stdin>:1:8:foobar foobaz
<stdin>:2:1:foobar

% print -rl 'foobar foobaz' foobar baz |
  \rg --color=never --no-heading --with-filename --line-number --column foo
<stdin>:1:1:foobar foobaz
<stdin>:2:1:foobar
```

Obviously then the column number corresponds to only the first match on the line.

Looking at [this](https://github.com/wsdjeg/SpaceVim/blob/cac5681a2c/autoload/SpaceVim/mapping/search.vim), it doesn't seem like you use the column number with any of your other search tools, so maybe you can just get rid of it entirely. On the face of it though it would seem ideal if your editor's search tool could take you to specific matches instead of just the first one on the line. idk, never used this

---

_Comment by @wsdjeg on 2018-07-06 12:58_

@okdana really thanks for your answer, that is what I needed.

@BurntSushi I am  so sorry I do not follow the issue template.

This issue can be closed, and I need to find same answer for ag, pt, ack, and grep.

---

_Closed by @wsdjeg on 2018-07-06 12:58_

---
