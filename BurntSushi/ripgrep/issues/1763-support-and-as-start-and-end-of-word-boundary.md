```yaml
number: 1763
title: "support \\< and \\> as start and end of word boundary markers"
type: issue
state: closed
author: jacob-keller
labels:
  - duplicate
assignees: []
created_at: 2020-12-15T23:44:12Z
updated_at: 2020-12-16T00:26:38Z
url: https://github.com/BurntSushi/ripgrep/issues/1763
synced_at: 2026-01-12T16:13:24Z
```

# support \< and \> as start and end of word boundary markers

---

_@jacob-keller_

#### Describe your feature request

Several regular expression tools such as grep, vim, emacs, git grep, sed, etc. support using '\<' as start of word boundary, and '\>' as end of word boundary. In order to obtain the exact behavior of '\<' and '\>', you would need to have some sort of lookahead or lookbehind syntax, such as in the perl regex engine to only match \b if it precedes or follows a word character.

It would be nice if ripgrep also supported use of \< and \> in order to inter-operate between other tools. I often think to use \< and \> when searching because I am used to the muscle memory of typing this in my editor and in other tools. But rg doesn't recognize this, so it results in the expression not working as intended.

Note that rg errors out when using "\<" or "\>" already:

```sh
rg "\<word\>"
regex parse error:
    \<word\>
    ^^
error: unrecognized escape sequence
```

So I don't think there would be a backwards compatibility issue. Previously any use of these escapes would cause a regex parse error, and once this is supported, they would become valid expressions.

---

_Comment by @jacob-keller on 2020-12-15 23:49_

Realized that use of \w was from someone explaining how to mimic the \< and \> with lookahead and lookbehind expressions, which aren't supported in rg. Woops.

---

_Comment by @BurntSushi on 2020-12-16 00:26_

Duplicate of https://github.com/BurntSushi/ripgrep/issues/872

It sounds like you use these a lot. Could you give more specifics on cases you use them where `\b` isn't good enough? I'm personally pretty skeptical of adding left and right word boundaries and generally don't think they carry their weight.

---

_Closed by @BurntSushi on 2020-12-16 00:26_

---

_Label `duplicate` added by @BurntSushi on 2020-12-16 00:26_

---
