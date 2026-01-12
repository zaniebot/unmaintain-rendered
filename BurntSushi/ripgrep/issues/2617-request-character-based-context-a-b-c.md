```yaml
number: 2617
title: "[Request] Character-based context (-A / -B / -C)"
type: issue
state: closed
author: mazunki
labels:
  - duplicate
assignees: []
created_at: 2023-10-02T20:05:43Z
updated_at: 2023-10-05T14:37:31Z
url: https://github.com/BurntSushi/ripgrep/issues/2617
synced_at: 2026-01-12T16:13:24Z
```

# [Request] Character-based context (-A / -B / -C)

---

_@mazunki_

It's not too uncommon for files to have huge lines containing some data, including your match. When grepping over a directory with tons of files, this can be quite overwhelming. 

_See first picture below._

There are some workarounds, but they all have some drawbacks:

- `-l` shows only matching files, with no context.
- `-M 24` is kind of useless by itself, as you might as well just use `-l` at that point
- `-M 24 --max-columns-preview` is partially useful, except it only displays the start of the line, and not the context of the match itself. Furthermore, `[... 0 more matches]` is kind of distracting. I feel this should be grayed out, at the very least (and hidden when there's no more matches).

_See second picture below._

My suggestion and request here is to extend the `--after-context` (`-A`), `--before-context` (`-B`) and `--context` (`-C`) to support character-based limits, instead of only working per-line, by the use of an optional period.

`rg -C=.24` would display 24 characters to the left, and 24 characters to the right. This could be combined as `-C=3.24` to display a context of 3 lines before and 3 lines after the match (also limited to the 24-character radius), although this is arguably not really useful in practice.

This would consider multiple matches in the same line as the center of a character range. If there's an overlap between both matches, it should consider it as one longer range, instead of duplicating the output for each case.

Example 1:
![image](https://github.com/BurntSushi/ripgrep/assets/32819373/4bc22105-ace3-4285-acad-c2ce8436cf67)

Example 2:
![image](https://github.com/BurntSushi/ripgrep/assets/32819373/1c74a317-9de7-42e7-848e-f4ecb913f370)

cc: @nate-sys

---

_Label `duplicate` added by @BurntSushi on 2023-10-05 14:36_

---

_Comment by @BurntSushi on 2023-10-05 14:37_

This is a duplicate of #1352.

If you want to copy your proposal in a comment on that issue, that would be great. I somewhat like your idea of reusing existing flags. But I haven't noodled on it too much. I'm not sure if it is too cute or has other issues.

---

_Closed by @BurntSushi on 2023-10-05 14:37_

---
