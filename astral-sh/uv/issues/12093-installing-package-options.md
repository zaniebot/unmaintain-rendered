```yaml
number: 12093
title: Installing package options
type: issue
state: closed
author: Barrowcroft
labels:
  - question
assignees: []
created_at: 2025-03-10T11:51:52Z
updated_at: 2025-04-09T16:08:14Z
url: https://github.com/astral-sh/uv/issues/12093
synced_at: 2026-01-12T16:00:55Z
```

# Installing package options

---

_@Barrowcroft_

### Question

I am installing pygubu-designer as a UV tool. I used to install pygubu-designer with an option to install additional widget sets using:

`pip install pygubu-designer[all]`

I don't seem to be able to find a UV equivalent of installing with "all"

`uv tool install pygubu-designer[all] `

doesn't work.

The only output is:

`zsh: no matches found: pygubu-designer[all]`

Can anyone help me 
thanks


### Platform

Mac OS Sequoia

### Version

uv 0.6.5 (Homebrew 2025-03-06)

---

_Label `question` added by @Barrowcroft on 2025-03-10 11:51_

---

_Comment by @konstin on 2025-03-10 11:54_

Can you please fill out the platform and version field in the template and share the log as described in #9452?

---

_Comment by @qartik on 2025-04-09 13:03_

I was running into the same issue, all I had to do was quote the package with the option.

```
❯ uv tool install "markitdown[all]"
```

Wish `uv` provided a better error message here.

```
❯ uv tool install markitdown[all]
zsh: no matches found: markitdown[all]

❯ uv --version
uv 0.6.13 (a0f5c7250 2025-04-07)
```

---

_Comment by @zanieb on 2025-04-09 13:28_

`zsh: no matches found: pygubu-designer[all]` is because your shell is recognizing `[...]` as a thing it should template. We can't provide a better message there, it's your shell that's erroring before uv is invoked.

---

_Comment by @qartik on 2025-04-09 14:07_

Fair, please feel free to close. Glad to find the root cause in any case.

---

_Comment by @Barrowcroft on 2025-04-09 14:52_

In that case how do I do the equivalent of 

`pip install pygubu-designer[all]`

using UV? It works fine normally.

---

_Comment by @qartik on 2025-04-09 15:05_

> In that case how do I do the equivalent of
> 
> `pip install pygubu-designer[all]`
> 
> using UV? It works fine normally.

Use `uv tool install "pygubu-designer[all]"`, notice the double quotes.

---

_Comment by @zanieb on 2025-04-09 15:48_

`pip install pygubu-designer[all]` will have the same error as `uv pip install pygubu-designer[all]` — your shell will error when you run the command.

You can use `uv pip install "pygubu-designer[all]"`

---

_Closed by @zanieb on 2025-04-09 15:48_

---
