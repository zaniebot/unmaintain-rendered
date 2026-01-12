```yaml
number: 293
title: "Using style \"bold\" does not use the bold ansi escape sequence in the output"
type: issue
state: closed
author: ilohmar
labels:
  - bug
assignees: []
created_at: 2016-12-26T16:56:30Z
updated_at: 2017-01-07T01:14:27Z
url: https://github.com/BurntSushi/ripgrep/issues/293
synced_at: 2026-01-12T16:13:21Z
```

# Using style "bold" does not use the bold ansi escape sequence in the output

---

_@ilohmar_

As @BurntSushi described in #51, color customization with the style "bold" is interpreted as "use a high-intensity color", therefore (when using a terminal) the actual ANSI escape sequence for bold text never appears in the output.

It would be great if this were an optional translation, such that "bold" really meant "output bold ANSI sequence" by default.

This is especially important when the output is parsed by a 3rd party that expects the color customization to work this way (as it does, e.g., for `git grep` or GNU `grep`).

---

_Comment by @BurntSushi on 2016-12-26 17:56_

Hmm, yes, I think this might be related to #266 (possibly even a dupe).

> This is especially important when the output is parsed by a 3rd party that expects the color customization to work this way

This sounds like a *really bad* idea. It would be better to not rely on colors to parse the output. My guess is that the right fix for that is #244, but I don't know for sure because I don't know what problem you're trying to solve.

---

_Label `bug` added by @BurntSushi on 2016-12-26 17:56_

---

_Comment by @ilohmar on 2016-12-26 18:15_

> This sounds like a really bad idea. It would be better to not rely on colors to parse the output.

I agree, but it might be a 31-year-old code base. ;-)  And the design will probably not change because it's targeting GNU grep (which does not offer another output format to my knowledge).

I see these issues discussed in #244 as well.  I will also look into the other issues discussing editor integration.  Thanks.

---

_Comment by @BurntSushi on 2016-12-26 18:19_

@ilohmar Unfortunately, it *can't* be a goal of ripgrep to be completely interface compatible with GNU grep (or any other grep) since it takes a fundamentally different view on how searching works by default. So you already need to be able to tweak the CLI invocation itself, and at that point, it seems like it's not unreasonable to expect to parse the output specially too. With that said, I can understand how using the color codes as a common interface would be tempting. I expect that in practice, once we nail out these last color issues, that stuff won't change going forward.

---

_Comment by @BurntSushi on 2017-01-07 01:14_

Fixed in https://github.com/BurntSushi/ripgrep/commit/b187c1a8175d88b743e2e225dbec98661f7a2f94

---

_Closed by @BurntSushi on 2017-01-07 01:14_

---
