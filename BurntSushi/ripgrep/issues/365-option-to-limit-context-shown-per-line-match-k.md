```yaml
number: 365
title: "option to limit context shown per line: match +- K columns)"
type: issue
state: closed
author: timotheecour
labels: []
assignees: []
created_at: 2017-02-14T23:57:35Z
updated_at: 2023-01-26T14:57:48Z
url: https://github.com/BurntSushi/ripgrep/issues/365
synced_at: 2026-01-12T16:13:21Z
```

# option to limit context shown per line: match +- K columns)

---

_@timotheecour_

@BurntSushi

sometimes rg shows lines and lines of wrapped text corresponding to a file:line match, this can occur eg for minified js files, html, or json files that are just 1 big line(s) of text.

This will obfuscate the relevant matches, will render slowly (and is even worse if, say, the command is run from a vertically split tmux pane because of the extra overhead in wrapping long lines: in that case also ^C or ^\ doesn't kill the rg process until much later).

of course a workaround is to exclude these files via `-g` but oftentimes this is undesirable and can be tedious depending on codebase (extension based filtering isn't good enough in my use cases)

Long story short: could we have:

```
--after-context-columns
--before-context-columns
--context-columns
```
which is analog of 

```
--after-context
--before-context
--context
```
but for horizontal, instead of vertical, context

Another useful option:
`--skip-line-greater-than=N`
which would skip a line greater than specified N columns or bytes


---

_Comment by @BurntSushi on 2017-02-15 01:15_

Dupe of #129

---

_Closed by @BurntSushi on 2017-02-15 01:15_

---

_Comment by @kdar on 2018-04-12 01:11_

Should we reopen this issue since the first part of it is still relevant and not fixed by --max-columns?

---

_Comment by @abitrolly on 2022-01-03 15:54_

Yes, I don't see how to extend `--only-matching` to include more context around.

---

_Comment by @benatkin on 2022-07-11 21:17_

Workaround and also a possible way of describing the behavior:

```
rg --only-matching '.{0,30}window.{0,30}'
```

Can still add a boundary:

```
rg --only-matching '.{0,30}\bdocument\b.{0,30}'
```

---

_Comment by @PanieriLorenzo on 2023-01-26 14:57_

please reopen this, it is not a duplicate of #129

---
