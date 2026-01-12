```yaml
number: 2012
title: Apply --text to some file types but not others
type: issue
state: closed
author: jpeloquin
labels:
  - wontfix
assignees: []
created_at: 2021-10-01T02:43:52Z
updated_at: 2023-11-24T20:15:40Z
url: https://github.com/BurntSushi/ripgrep/issues/2012
synced_at: 2026-01-12T16:13:24Z
```

# Apply --text to some file types but not others

---

_@jpeloquin_

_Summary_: To ensure search of files that are mostly text while still excluding fully-binary files, it would be helpful to be able to declare that certain file types should be treated as text even when null bytes are present.

_Motivation_: Ripgrep currently uses a simple heuristic to detect binary files, for a variety of good reasons.  Still, this heuristic sometimes excludes files of interest that are mostly text, such as texinfo or log files.  Since these file types are _often_ 100% text and always _mostly_ text, the user may not realize that a subset of their files contain null bytes and are affected by the binary exclusion heuristic.  When it is critical to avoid unwanted exclusions, the user can use `--text` as a precaution, but the resulting inclusion of every genuinely binary file can (a) slow the search by including extra data and (b) return many junk results.  Applying --text to specific file types of interest would mitigate these disadvantages.

Note that workarounds exist: the user can search once with the default heuristic, `rg foo`, and a second time with the mostly-text file types of interest, `rg --text -g '*.info foo` or `rg --text --type all foo`.  (The latter won't include extensionless files like README so it doesn't suffice on its own.)  So the request is just a minor quality-of-life increase; if the implementation would be complex it might not be worth it.

_Tentative syntax_: If this request is favorable in principle, I tentatively suggest two possibilities for syntax (probably pick only one):

1. Extend `--type-add`'s `TYPE_SPEC` pattern to take a text declaration, like `--type-add 'log:*log type=text'`.  Relative to the following option, this is harder to use on an ad-hoc basis.
2. Extend the `--text` flag to take a TYPE pattern, like `--text bar`. (Presumably the user would define a type `bar` with everything they want always treated as text.)  This has the disadvantage that the existing use, `--text`, has no type pattern (a missing type is not equivalent to "all") and would have to be special-cased for compatibility.

The syntax isn't that important though and I'm certainly not the best person to choose it.  Regardless, the user would presumably use the new arguments in their .ripgreprc to make sure certain file types of interest are always searched.  Much like .gitattributes is used to configure whether git diff treats certain file types as binary or text.

---

_Comment by @BurntSushi on 2023-11-24 20:15_

I'm going to pass on this. I think it's too subtle of a behavior.

---

_Closed by @BurntSushi on 2023-11-24 20:15_

---

_Label `wontfix` added by @BurntSushi on 2023-11-24 20:15_

---
