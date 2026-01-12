```yaml
number: 663
title: "Glob: support for [^...] notation"
type: issue
state: closed
author: bpasero
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2017-11-06T10:09:30Z
updated_at: 2017-11-22T11:56:04Z
url: https://github.com/BurntSushi/ripgrep/issues/663
synced_at: 2026-01-12T16:13:22Z
```

# Glob: support for [^...] notation

---

_@bpasero_

It looks like RipGrep is already supporting the `[^...]` syntax for glob patterns which will match on a character-range with negation. Can the `[!...]` syntax also be supported for the same purpose?

From http://man7.org/linux/man-pages/man7/glob.7.html
 
> An expression "[!...]" matches a single character, namely any
> character that is not matched by the expression obtained by removing
> the first '!' from it.  (Thus, "[!]a-]" matches any single character
> except ']', 'a' and '-'.)

---

_Renamed from "Glob: support for [!...] notation" to "Glob: support for [^...] notation" by @bpasero on 2017-11-06 10:16_

---

_Comment by @bpasero on 2017-11-06 10:16_

Sorry, I got confused. RipGrep is supporting `[!...]` but actually not `[^...]` which for example node-glob seems to [support](https://github.com/isaacs/node-glob#glob-primer). 

I am however not finding a standard that describes the `[^...]` syntax, so feel free to close.

---

_Comment by @BurntSushi on 2017-11-06 10:28_

Your second comment has it right.

---

_Closed by @BurntSushi on 2017-11-06 10:28_

---

_Comment by @okdana on 2017-11-06 23:03_

FYI, even though POSIX (2.13.1) leaves the behaviour undefined, most glob-pattern implementations actually *do* support `[^...]` in addition to `[!...]`. bash does; zsh does; `fnmatch(3)` does in most C libraries, including GNU, FreeBSD/macOS, OpenBSD, and even musl; and, perhaps most importantly, [git does too](https://github.com/git/git/blob/7668cbc60578f99a4c048f8f8f38787930b8147b/wildmatch.c#L17).

Something worth considering?

---

_Reopened by @BurntSushi on 2017-11-06 23:21_

---

_Comment by @BurntSushi on 2017-11-06 23:21_

@okdana Cool, I didn't know that! I'm hopeful that it is a relatively simple addition to the `globset` crate. It is technically a breaking change, but I think we're fine. (`globset` should get a semver bump at least though.)

---

_Label `enhancement` added by @BurntSushi on 2017-11-06 23:22_

---

_Label `help wanted` added by @BurntSushi on 2017-11-06 23:22_

---

_Closed by @BurntSushi on 2017-11-22 11:56_

---
