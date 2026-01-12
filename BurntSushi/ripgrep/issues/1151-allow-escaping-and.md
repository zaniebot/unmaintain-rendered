```yaml
number: 1151
title: "Allow escaping < and >"
type: issue
state: closed
author: mqudsi
labels:
  - invalid
assignees: []
created_at: 2018-12-30T15:02:35Z
updated_at: 2018-12-30T15:48:08Z
url: https://github.com/BurntSushi/ripgrep/issues/1151
synced_at: 2026-01-12T16:13:23Z
```

# Allow escaping < and >

---

_@mqudsi_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

`cargo`

#### What operating system are you using ripgrep on?

FreeBSD 12

#### Describe your question, feature request, or bug.

Depending on the particular variant of regex used, it is sometimes needed to escape a literal `<` and `>` in a regex and sometimes not. `rg` does not use named captures and thus `>` and `<` are not considered special characters that need to be escaped in its regex syntax.

If you write pcre2-ish regexes by default, you may be accustomed to escaping these literals. If you do so, ripgrep errors out:

```
mqudsi@ZBOOK /m/c/U/m/r/a/t/debug> rg '\<'
regex parse error:
    \<
    ^^
error: unrecognized escape sequence
```

May I humbly request that `\>` and `\<` be treated as legal escapes evaluating to `>` and `<` for compatibility purposes?

---

_Comment by @BurntSushi on 2018-12-30 15:16_

> `rg` does not use named captures and thus `>` and `<` are not considered special characters that need to be escaped in its regex syntax.

ripgrep does support named captures:

```
$ echo foo123bar | rg '(?P<number>[0-9]+)' -or 'the number: $number'
the number: 123
```

However, neither `<` nor `>` need to be escapeable because they can only appear after a specific sequence of characters (namely, `(?P`), where both `(` and `?` are escapeable.

With that said, I'm going to pass on this for two reasons:

1. This is a property of the underlying regex engine, not ripgrep, and I have generally remained quite conservative with respect to escape sequences in order to make it possible to expand the syntax in a backwards compatible manner.
2. Incidentally, `\<` and `\>` do actually have special meaning in some regex engines that correspond to zero width assertions for left and right word boundaries, respectively. There is even a [tracking ticket](https://github.com/rust-lang/regex/issues/469) on the regex engine to consider supporting them. If the regex engine added support for nominal escapes of `<` and `>`, then the left/right word boundary feature couldn't be added without a backwards incompatible change.

> for compatibility purposes?

In the future, please include **specific concrete use cases**. "compatibility purposes" is not particularly compelling to me in isolation.

---

_Closed by @BurntSushi on 2018-12-30 15:16_

---

_Label `invalid` added by @BurntSushi on 2018-12-30 15:16_

---

_Comment by @mqudsi on 2018-12-30 15:48_

Thank you.

---
