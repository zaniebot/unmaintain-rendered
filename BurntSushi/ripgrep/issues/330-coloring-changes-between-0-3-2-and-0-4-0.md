```yaml
number: 330
title: Coloring changes between 0.3.2 and 0.4.0?
type: issue
state: closed
author: scottchiefbaker
labels: []
assignees: []
created_at: 2017-01-17T22:00:06Z
updated_at: 2017-01-18T00:34:35Z
url: https://github.com/BurntSushi/ripgrep/issues/330
synced_at: 2026-01-12T16:13:21Z
```

# Coloring changes between 0.3.2 and 0.4.0?

---

_@scottchiefbaker_

On Linux using the 0.3.2 and 0.4.0 binary releases from Github produce different colors in the output. My rg alias is:

    alias rg='rg --smart-case --colors line:fg:yellow --colors path:fg:green'

![rg-colors](https://cloud.githubusercontent.com/assets/3429760/22041734/dbcc2538-dcbc-11e6-8914-a7941b0a860d.png)

Note the green and yellow are darker, and the red is brighter. Did the spec for `--colors` change between versions? I don't see any mention of this in the docs. Perhaps this is somehow related to #182?

---

_Comment by @BurntSushi on 2017-01-18 00:32_

The spec for `--colors` did not have any breaking changes, but it does look like I missed commit b187c1 from the CHANGELOG.

Firstly, the default colors/styles changed. File paths went from green to magenta, line numbers went from blue to green and matches stayed as red. Both paths and line numbers are no longer bolded, but matches remain bolded. This color scheme generally matches GNU grep's color scheme.

Secondly, I fixed a bungling of intense colors. Namely, if a bold color was specified, then an intense color was used instead of emitting the bold ANSI escape sequence. Therefore, I added a new style `intense` to distinguish between the colors.

You can try fiddling with intensity and boldness like this:

```
$ rg --colors path:style:intense --colors line:style:bold
```

---

_Closed by @BurntSushi on 2017-01-18 00:34_

---
