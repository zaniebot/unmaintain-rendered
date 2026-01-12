```yaml
number: 2677
title: "Inline flag (?x) with comment doesn't work due to automatic wrapping of patterns inside non-capturing group"
type: issue
state: open
author: learnbyexample
labels:
  - bug
assignees: []
created_at: 2023-12-06T10:59:43Z
updated_at: 2023-12-06T13:08:41Z
url: https://github.com/BurntSushi/ripgrep/issues/2677
synced_at: 2026-01-12T16:13:24Z
```

# Inline flag (?x) with comment doesn't work due to automatic wrapping of patterns inside non-capturing group

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

Using `(?x)` to add comments isn't working due to automatic wrapping of the supplied pattern within `(?:)`.

### What are the steps to reproduce the behavior?

Any pattern which uses `(?x)` and a comment.

### What is the actual behavior?

```bash
$ echo 'fig a#b 123' | rg -o '(?x)\d+ #extract digits'
regex parse error:
    (?:(?x)\d+ #extract digits)
    ^
error: unclosed group
```

### What is the expected behavior?

I think automatic wrapping of patterns inside non-capturing group was introducted for this issue: https://github.com/BurntSushi/ripgrep/issues/2480. If possible, don't modify the pattern when there's a comment. Or may be add a note in the documentation for this corner case.

FWIW, `GNU grep` doesn't support multiple patterns via `-e` or `-f` for PCRE:

```bash
$ echo 'fig a#b 123' | grep -oP '(?x)\d+ #extract digits'
123

$ echo 'MyText' | grep -P -e '(?i)notintext' -e 'text'
grep: the -P option only supports a single pattern
```

---

_Label `bug` added by @BurntSushi on 2023-12-06 12:41_

---

_Comment by @BurntSushi on 2023-12-06 13:08_

There's really no way to get this completely right without individually validating each regex. I very specifically wanted to avoid doing that because of the extra cost it would add. With that said, now that I'm thinking about it more, it might be possible to do that with a fast path that skips the check in most cases. (For example, if a pattern doesn't contain a meta character anywhere, then you know it's valid. That would help when searching large dictionaries of literals.)

But yeah, when I added a fix for #2480, I knew it wouldn't work in all cases. Another example of this is `rg ')('`. That should be invalid, but it actually works.

With that said, one thing I realized here is that by adding the group around the regex, that group (which the user did not type) now appears in error messages. That's bad juju and I'll need to revert that for that reason alone. When I do that, I'll look into validating each pattern manually.

For PCRE2, we'll probably just need to accept a `wontfix` bug there. I don't see a good reason to completely disable multi-pattern matching there just for this reason.

---
