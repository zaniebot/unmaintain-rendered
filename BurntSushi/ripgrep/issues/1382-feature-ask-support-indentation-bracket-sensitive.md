```yaml
number: 1382
title: "Feature Ask: Support indentation/bracket sensitive grepping as a mode"
type: issue
state: closed
author: nojvek
labels: []
assignees: []
created_at: 2019-09-18T19:27:05Z
updated_at: 2025-07-12T05:31:23Z
url: https://github.com/BurntSushi/ripgrep/issues/1382
synced_at: 2026-01-12T16:13:23Z
```

# Feature Ask: Support indentation/bracket sensitive grepping as a mode

---

_@nojvek_

Ripgrep is pretty phenomenal and I love it. Also thanks to @roblourens + vscode folks that it's also used as the search engine for vscode with a great UX.

Now I fully realize that regexes are context insensitive grammars and to keep track of indents/brackets, you need some sort of a parse stack.

But here's what I want to do. I want to find all occurences of `choices=` inside of `models\..*?\( ... \)`
```py

    status = models.CharField(
        max_length=10,
        choices=[(SINGLE, SINGLE), (MARRIED, MARRIED), (DIVORCED, DIVORCED)],
        default=SINGLE,
        null=False,
    )

...
    assert(len(status.choices), 3)
```

if I have `models\..*?\((.|\n)+(choices=)(.|\n)+\)` which would do a multiline match but also end up matching status.choices


This could be a mode. I guess if the whole indentation business is hard to tackle, even a bracket only mode would be awesome such that match only occurs if opening and closing brace are on same count. ripgrep would assign a level count to `(){}[]<>` characters.

so you could say `models\.CharField\(<special selector may be?>\)` and it would match the whole block rather than just stop at the first closing parenthesis 

```
models.CharField(
        max_length=10,
        choices=[(SINGLE, SINGLE), (MARRIED, MARRIED), (DIVORCED, DIVORCED)],
        default=SINGLE,
        null=False,
    )
```



---

_Comment by @BurntSushi on 2019-09-18 20:31_

I'm going to have to decline, sorry. This sounds more like a specialized tool to me, which someone can build using ripgrep's libraries.

---

_Closed by @BurntSushi on 2019-09-18 20:31_

---

_Comment by @JJK96 on 2025-07-12 05:31_

For future reference, you can do this with the supported PCRE syntax. It supports recursive regexes: https://www.regular-expressions.info/recurse.html

---
