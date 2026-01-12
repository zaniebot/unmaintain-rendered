```yaml
number: 599
title: Incorrect result display with empty line and colors
type: issue
state: closed
author: roblourens
labels: []
assignees: []
created_at: 2017-09-06T21:40:38Z
updated_at: 2017-10-22T02:40:43Z
url: https://github.com/BurntSushi/ripgrep/issues/599
synced_at: 2026-01-12T16:13:22Z
```

# Incorrect result display with empty line and colors

---

_@roblourens_

https://github.com/Microsoft/vscode/issues/31252

When I search `^$`, I get, for each file, a correctly formatted result line for the first empty line in the file, but the other result lines have extra color escape codes for a 'match' before the line number.

The command I'm using to demonstrate is `rg --color ansi --colors 'path:none' --colors 'line:none' --colors 'match:fg:red' --colors 'match:style:nobold' --heading --line-number '^$' > out.txt` - the extra `--colors` flags are not necessary to the repro, they just make it a little easier to see the issue. And this might be a good time to ask whether you've made any progress towards https://github.com/BurntSushi/ripgrep/issues/244 😁 

Result
```
^[[mtest.txt^[[m
^[[m2^[[m:^[[m^[[31m^[[m
^[[m^[[31m^[[m^[[m4^[[m:^[[m^[[31m^[[m
^[[m^[[31m^[[m^[[m6^[[m:^[[m^[[31m^[[m
^[[m^[[31m^[[m^[[m8^[[m:^[[m^[[31m^[[m
```

You can see the first line starts with the `2:` surrounded by escapes. But other lines first have escape codes for a match, then `4:`, then another set of codes for a match.

Expected:
I search another file for `^foo$`.

```
^[[mtest2.txt^[[m
^[[m2^[[m:^[[m^[[31mfoo^[[m
^[[m4^[[m:^[[m^[[31mfoo^[[m
^[[m6^[[m:^[[m^[[31mfoo^[[m
^[[m8^[[m:^[[m^[[31mfoo^[[m
```

---

_Closed by @BurntSushi on 2017-10-22 02:40_

---
