```yaml
number: 1991
title: Reflow help text to fit within 80 columns
type: pull_request
state: closed
author: mattiase
labels: []
assignees: []
base: master
head: reflow-help
created_at: 2021-09-16T10:57:46Z
updated_at: 2023-07-09T10:34:37Z
url: https://github.com/BurntSushi/ripgrep/pull/1991
synced_at: 2026-01-12T18:23:14Z
```

# Reflow help text to fit within 80 columns

---

_@mattiase_

The `--help` text was unnecessarily hard to read in an 80 column terminal window (which is the default on many systems, for reasons good and bad). This change reflows the paragraphs so that they fit.

As the text is printed with 12 spaces indentation, the maximum paragraph line length is 68 characters. Some people prefer not using the last (80th) column because older terminals may have trouble showing such lines without wrapping; I didn't care about that but if you do, I'll readjust.

Similarly, there are good reasons for using even shorter text lines in prose; `man` leaves a few more columns empty at the right margin for reasons of legibility and aesthetics. That might be a good idea here, too.

I don't think full justification in monospaced text is, well, justified but again some people like it -- do tell.


---

_Comment by @mattiase on 2021-09-19 10:29_

It appears that the `-h` help text would benefit from a similar treatment but before I extend the PR to handle that, it would be good to know that we agree on the basics to avoid wasting your time and mine.


---

_Comment by @BurntSushi on 2023-07-08 18:31_

I think clap is supposed to do this automatically.

Either way, I'd prefer we not strongly couple---where reasonable---the line length limits shown in the `--help` output with the line length limits in the source code. This, for example, is something I'd guess would be difficult to maintain. It's already hard to get people to adhere to 79 columns, even though the surrounding code/text clearly respects it. This is just going to be a nightmare to enforce and basically impossible to see clearly in PR review.

---

_Closed by @BurntSushi on 2023-07-08 18:31_

---

_Comment by @mattiase on 2023-07-08 19:55_

> I think clap is supposed to do this automatically.

Right, and I agree it's better than fixed formatting. But clap doesn't work properly without the `wrap_help` feature; see the bottom comments of issue #442.
I build ripgrep with `wrap_help` added to `Cargo.toml` locally and it works. Shall I open a new PR? It's only a one-liner, so perhaps you prefer doing it yourself.


---

_Branch deleted on 2023-07-08 19:55_

---

_Comment by @BurntSushi on 2023-07-08 21:21_

Yes, but that adds a new dependency on `term_size` which appears deprecated.

I was thinking about this: https://github.com/BurntSushi/ripgrep/blob/f4d07b9cbdcf7d52e63f35fe76279e99a17492ef/crates/core/app.rs#L63

But it's set to 100.

I am currently thinking about migrating away from clap to lexopt, and if and when I do that, I'll reconsider this. I just know that the approach taken in this PR isn't going to work for me.

---

_Comment by @mattiase on 2023-07-09 09:44_

We could also add backslashes to the end of lines in doc strings, so that clap's attempt to word-wrap actually works. It's actually already done in places but not very consistently.

It's a bit of work but since I already did it in my tree, there's no extra cost. New PR?


---

_Comment by @BurntSushi on 2023-07-09 09:50_

That doesn't sound like a good idea to me at all. As I said, the strings in the code should be optimized for reading and writing in the code. Adding backslashes everywhere sounds bad for both reading and writing.

Whatever solution is used here needs to leave the doc strings in the code as they are.

---

_Comment by @mattiase on 2023-07-09 10:34_

Right. Then new text filling code is needed, that uses blank lines to detect end of paragraphs, perhaps along with some rudimentary mark-up to force line breaks.

There are a couple of places where an indented block is used (such as the warning paragraph in the doc string for `--pre`); some mark-up would be needed there too, to distinguish it from pre-formatted text. And there are some tables.

These could all be treated as preformatted to simplify the text filler, but then their strings have to be formatted by hand to fit some typical narrowest case (probably 80 columns).

All doable.


---
