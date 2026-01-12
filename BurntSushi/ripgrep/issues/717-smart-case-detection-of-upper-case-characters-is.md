```yaml
number: 717
title: "--smart-case detection of upper-case characters is unintuitive"
type: issue
state: closed
author: okdana
labels:
  - bug
assignees: []
created_at: 2017-12-17T22:51:46Z
updated_at: 2018-03-10T05:11:32Z
url: https://github.com/BurntSushi/ripgrep/issues/717
synced_at: 2026-01-12T16:13:22Z
```

# --smart-case detection of upper-case characters is unintuitive

---

_@okdana_

The `--smart-case` option doesn't behave the way i expect in terms of determining whether the pattern contains upper-case characters. Specifically, a search like the following will fail:

```
rg -S 'foo\w' <<< FOOBAR
```

This is because it expands the class `\w` and discovers that it contains the range of upper-case characters `A-Z`.

`ag` uses a different metric: It simply checks the input pattern byte by byte to see if there's an upper-case character in the ASCII range. This works out in the `foo\w` case, but it will fail in the similarly obvious case of `foo\S` (which `rg` happens to get right) — not to mention multi-byte stuff like `É` and `ß`.

There are also more complicated cases where they both fail in different ways, like this:

```
rg -S '\p{Ll}' <<< FOOBAR
```

`rg` returns a match here, because `\p{Ll}` expands to only lower-case characters. But i think this should be treated as an 'explicit' range, like `[A-Z]` would be.

At the very least, though, i feel like something as basic as `foo\w` should probably work as expected with smart case.

Is there any way to determine through the AST the actual input that produced a class? Or would that be easy to add? I'm not sure i'm at the level yet where i should be messing with the `regex` crate, but that seems like it'd be a convenient way to sort it.

The easiest 'good enough' alternative that comes to mind would be to do a semi-naïve scan of the pattern that's just smart enough to ignore stuff like short-hand classes, but that feels kind of dumb (especially since i get the impression that you don't really care for this feature in the first place...).

---

_Comment by @BurntSushi on 2017-12-18 12:11_

> (especially since i get the impression that you don't really care for this feature in the first place...)

Haha yeah I am not a fan of it, but recognize that others like it. It should definitely work intuitively, and I basically agree with your analysis here.

I think you are on the right track with respect to fixing this. The problem is that the regex "AST" isn't a true faithful AST. The parser itself does various transformations that cannot be reversed, so there is no way to "see" that a `\w` was actually written as a `\w` instead of a `[...]`.

I have been working on a rewrite of the parser for quite some time now, and with it, a [proper AST](https://github.com/rust-lang/regex/blob/869d0067fe70e535607d02b7474eb0ed6c83c43c/regex-syntax-2/src/ast.rs). Once that is active (I'm not sure when that will be), then we can scan the AST and apply this heuristic with as much precision as we care to.

---

_Label `bug` added by @BurntSushi on 2017-12-18 12:11_

---

_Comment by @BurntSushi on 2017-12-18 12:12_

> The easiest 'good enough' alternative that comes to mind would be to do a semi-naïve scan of the pattern that's just smart enough to ignore stuff like short-hand classes

@okdana If you wanted to do this in the shorter term to get at least some simple cases right, then I'd be in favor of this. I believe the check is done in the `grep` crate in this repo.

---

_Closed by @BurntSushi on 2017-12-18 22:58_

---

_Comment by @BurntSushi on 2018-03-10 04:01_

@okdana With the new AST coming in soon, what do you think about this set of rules for smart case?

When the `--smart-case` flag is given, ripgrep chooses to match case insensitively if and only if there is at least one literal character in the pattern and that all such literals are not considered as uppercase. If the pattern contains no literals or at least one uppercase literal character, then normal case sensitive search is used.

Examples: `abc`, `[a-z]`, `abc\w` and `abc\p{Ll}` are all searched case insensitively while `aBc`, `[A-Z]`, `aBc\w` and `\p{Ll}` are searched case sensitively.

---

_Comment by @okdana on 2018-03-10 05:11_

Hmm. I can't think of any case where that wouldn't make sense. Yeah, sounds cool.

---
