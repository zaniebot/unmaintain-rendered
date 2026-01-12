```yaml
number: 1843
title: Add support for the LS_COLORS environment variable
type: issue
state: closed
author: noib3
labels:
  - wontfix
assignees: []
created_at: 2021-04-03T13:54:36Z
updated_at: 2021-04-03T15:27:45Z
url: https://github.com/BurntSushi/ripgrep/issues/1843
synced_at: 2026-01-12T16:13:24Z
```

# Add support for the LS_COLORS environment variable

---

_@noib3_

It would be nice to have support for the `LS_COLORS` variable used by various other programs (`fd` and `lf` come to mind) to highlight file paths with different colors depending on their type.

The following screenshot compares the output of `rg`, which currently uses a static color (magenta?) for all files, versus `fd`, which supports `LS_COLORS`, in a directory with a markdown file, a rust file and a pdf:

![2021-Apr-03@15:37:26](https://user-images.githubusercontent.com/59321248/113480488-ba80b580-9494-11eb-8d46-9cb681d92e6a.png)


---

_Comment by @BurntSushi on 2021-04-03 13:59_

ripgrep supports its own platform independent way of specifying colors: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#colors

There are no plans to add any other mechanism to configure colors.

---

_Closed by @BurntSushi on 2021-04-03 13:59_

---

_Label `wontfix` added by @BurntSushi on 2021-04-03 13:59_

---

_Comment by @noib3 on 2021-04-03 14:15_

Fair enough, but the current mechanism is quite limiting as it only allows specifying colors for the path *as a whole*, with no distinction for different file extensions (e.g. pdf files vs rust files) or different path components (e.g. parent directory vs file name). 

This wouldn't be a replacement of the current colors implementation (it only applies to the `path` `{style}`), just a nice option to have.

---

_Comment by @BurntSushi on 2021-04-03 15:27_

> only allows specifying colors for the path _as a whole_

This is a bit tangled. The current _mechanism_ doesn't prevent this. It's just there there is no color type (or indeed, any configuration inside of ripgrep) that permits this granularity of color control. But this has nothing to do with `LS_COLORS` support.

Otherwise, yes, I'm okay with that. I'm not too interested in giving super granular control over colors.

So it kind of sounds like you got two separate issues tangled up here. One of them is "I want a different way to specify colors." The other is, "I want more control over the color of different parts of the output." Incidentally, I think my answer is "no" to both.

---
