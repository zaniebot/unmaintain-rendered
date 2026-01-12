```yaml
number: 1496
title: Different coloring for context lines
type: issue
state: open
author: jaroslawr
labels:
  - enhancement
assignees: []
created_at: 2020-02-22T10:17:41Z
updated_at: 2020-02-22T14:19:59Z
url: https://github.com/BurntSushi/ripgrep/issues/1496
synced_at: 2026-01-12T16:13:23Z
```

# Different coloring for context lines

---

_@jaroslawr_

It would be nice to be able to specify different coloring for context lines - lines that do not contain a match but come from application of `-A/-B/-C` options, e.g. `rg --color nomatchline:fg:gray,line:fg:white`. This is possible in vanilla grep like this:

```
GREP_COLORS="cx=37:" grep -R foo
```

---

_Comment by @BurntSushi on 2020-02-22 12:58_

Can you specify the desired behavior when `-v` is used and matches in the context lines are highlighted? e.g., This is how ripgrep behaves now:

![context-color](https://user-images.githubusercontent.com/456674/75092691-4a40fa00-5548-11ea-8738-17576e01ccfe.png)

In the second example, `bar` is highlighted as a _match_ even though it is in a non-matching context line. 

I guess it looks like grep combines them?

![context-color-grep](https://user-images.githubusercontent.com/456674/75092751-fa166780-5548-11ea-98a4-e7d129a72592.png)

Not quite sure how I feel about adding yet another color option though. Although I can see how "graying" out the context could lead to a nice effect.

---

_Label `enhancement` added by @BurntSushi on 2020-02-22 12:58_

---

_Comment by @jaroslawr on 2020-02-22 14:19_

`git grep` also has the feature in question and has more understandable options than `GREP_COLORS`, it provides these color settings:
- `match` matching text (same as setting matchContext and matchSelected)
- `matchSelected` matching text in selected lines
- `matchContext` matching text in context lines (I guess covers the `-v` case you described)
- `selected` non-matching text in selected lines
- `context` non-matching text in context lines
(per https://git-scm.com/docs/git-config, under "color.grep" section)

Taking inspiration from this, I think `ripgrep` could allow to set one or both of new options: `matchSelected` and `matchContext`, which when set would take precedence over the `match` option. Analogously it could allow to set `lineSelected` and `lineContext`, taking precedence over `line` when set.

---
