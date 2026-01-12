```yaml
number: 1842
title: Add option to customize the separator character betwen paths, lines, columns and matches
type: issue
state: closed
author: noib3
labels:
  - enhancement
  - help wanted
  - rollup
assignees: []
created_at: 2021-04-03T13:47:13Z
updated_at: 2022-07-19T09:54:55Z
url: https://github.com/BurntSushi/ripgrep/issues/1842
synced_at: 2026-01-12T16:13:24Z
```

# Add option to customize the separator character betwen paths, lines, columns and matches

---

_@noib3_

As of version 12.1.1, if the `--no-heading` option is used then a `:` will be used to separate paths, lines, columns (if the `--column` option is used) and matches. Is it possible to customize those characters individually?

For example I'd like to use a `@` between paths and lines, the default `:` between lines and columns and a space between columns and matches.

I'm new to ripgrep, I've read the manual but I haven't found anything about it so I assume this behaviour is not currently supported?

---

_Comment by @BurntSushi on 2021-04-03 13:57_

There is no way to customize it. But the underlying library does support customizing it: https://github.com/BurntSushi/ripgrep/blob/7923d252285d78b023a237d669d741dfa210ff86/crates/core/args.rs#L789-L790

I think I would be OK with a PR that adds two flags, one each for customizing the match and context separators for fields emitted in each case.

As a hint, you'll want to use [`cli::unescape_os`](https://github.com/BurntSushi/ripgrep/blob/7923d252285d78b023a237d669d741dfa210ff86/crates/core/args.rs#L1025) to extract out the separator from the CLI argument.

---

_Label `enhancement` added by @BurntSushi on 2021-04-03 13:57_

---

_Label `help wanted` added by @BurntSushi on 2021-04-03 13:57_

---

_Comment by @samwho on 2021-04-09 10:35_

I'm also keen to have this feature, and my use-case is to make sure that the file path and line number are presented in the terminal in a way that can be ctrl+left clicked such that they will open to that line in VSCode.

I use the following settings:

```
--max-columns=150
--max-columns-preview
--smart-case
--search-zip
--color=auto
--no-heading
--no-messages
--trim
```

And the output looks like this:

```
$ rg use
src/entries.rs:186:use super::*;
src/entries.rs:187:use std::io::Cursor;
src/entries.rs:188:use test_case::test_case;
```

Sometimes, the fact that there is no space after the 2nd colon on each line makes it such that VSCode won't open the file at that line, instead it will initiate a search that yields nothing. Having a space there makes this work 100% of the time.

Removing `--trim` isn't a solution.

---

_Comment by @samwho on 2021-04-09 18:04_

I've forked the repo and started taking a preliminary look. If I manually change line 789 of `ripgrep/crates/core/args.rs` to be a space instead of a colon, it doesn't quite do what I'd like. I'd like to be able to keep the colon between the path and the line number, but have a space between the line number and the match.

Would you be open to accepting a PR that changes the `printer` crate to allow this?

---

_Comment by @BurntSushi on 2021-04-09 18:12_

@samwho Possibly. It depends on if there are any performance problems created by accepting multiple bytes vs one byte. It's been a while since I've touched that code, but my instinct is that there shouldn't be.

---

_Label `rollup` added by @BurntSushi on 2021-05-31 01:01_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---

_Comment by @fallaciousreasoning on 2022-07-18 23:11_

@samwho did you come up with a solution? I have the same problem with VSCode link detection, and `--field-match-separator " "` doesn't quite do what I want, as I lose the line number when clicking a link.

---

_Comment by @samwho on 2022-07-19 09:54_

I'm afraid I didn't.

---
