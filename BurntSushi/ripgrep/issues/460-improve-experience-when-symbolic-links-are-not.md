```yaml
number: 460
title: Improve experience when symbolic links are not searched
type: issue
state: closed
author: lzybkr
labels:
  - enhancement
assignees: []
created_at: 2017-04-26T01:43:23Z
updated_at: 2017-10-22T01:48:06Z
url: https://github.com/BurntSushi/ripgrep/issues/460
synced_at: 2026-01-12T16:13:22Z
```

# Improve experience when symbolic links are not searched

---

_@lzybkr_

Today, there is an inconsistent experience around symbolic links.

If no files are searched (e.g. you are in a directory that only contains symbolic links), you get an error message suggesting `--debug`, but that doesn't help.

If at least 1 file is searched, you get no output even if you should have.

If no files are searched, it might be worth adding some text to the error suggesting `-L` or `--follow`.
If some files are searched, it might be nice to have some way of listing which files/directories were skipped - e.g. because they were links, binary files, excluded by .gitignore, hidden, or whatever.

---

_Comment by @BurntSushi on 2017-04-26 02:02_

It sounds like you're asking for symbolic links that were skipped to appear in the output of --debug? Would that resolve things for you?

> If at least 1 file is searched, you get no output even if you should have.

I don't understand the issue here. Could you say more and also say how you expect it to be solved?

Printing files that were skipped by default is a non-starter IMO.

---

_Comment by @lzybkr on 2017-04-26 03:27_

What I meant was - sometimes you search, sometimes there is no output, but in your mind, there should have been. Most likely it's user error, but it could be a bug in ripgrep. Either way, I want help understanding what happened.

Ultimately I'm suggesting that ripgrep should provide an option to explain what was skipped and why so that I, as a user, can understand exactly what was and wasn't searched.

This definitely can't be a default - there would be too much noise.

The explanation should include the why if possible - filter, .gitignore, sym link, binary, others?

---

_Label `enhancement` added by @BurntSushi on 2017-04-26 11:45_

---

_Comment by @BurntSushi on 2017-04-26 11:48_

@lzybkr Right, OK. That is indeed the problem I was trying to solve with the output in `--debug`, and you're right to point out that not everything that's skipped is there and the output can certainly be improved.

I don't think it can ever be quite perfect. For example, if you know there is a match in the file `foo/bar/baz/quux/something` but `foo/bar` was skipped, then the `--debug` output isn't going to say that `foo/bar/baz/quux/something` was skipped specifically, but rather, that `foo/bar` was skipped. So there's still a bit of a mismatch there... One supposes that the presence of the `--debug` flag could force ripgrep to do extra work for the sake of printing more stuff, but that's likely to add implementation complexity that I'm not sure I want.

---

_Comment by @BurntSushi on 2017-10-22 01:48_

I think I'm going to close this because I'm not sure what the concrete steps would be to satisfy this request. Additionally, I kind of feel like "the tool doesn't follow symlinks by default but has a flag to enable it" is pretty standard for command line tools (e.g., `grep` and `find`). On top of that, ripgrep provides the `--files` flag, which tells you exactly which files it will search, which is usually enough to check whether ripgrep is doing what you think it's doing.

---

_Closed by @BurntSushi on 2017-10-22 01:48_

---
