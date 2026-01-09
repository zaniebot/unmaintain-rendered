---
number: 13443
title: "feature: per file line-length.  "
type: issue
state: closed
author: toppk
labels:
  - duplicate
assignees: []
created_at: 2024-09-22T00:36:35Z
updated_at: 2024-09-27T09:39:19Z
url: https://github.com/astral-sh/ruff/issues/13443
synced_at: 2026-01-07T13:12:15-06:00
---

# feature: per file line-length.  

---

_Issue opened by @toppk on 2024-09-22 00:36_

using ruff 0.5.1 I don't think there's a way to have a different line-length setting per file.  I can disable E501, but I cannot tweak it, for the occasional file the just has a lot of little bit too long lines.  like I'm okay with 100 instead of 88, but not 120,     this would be really hard to do in pyproject.toml, but I'm hoping some at least we could get a pragma for the top of a file.  e.g. `# ruff: setting: line-length: 98`


this is especially annoying working with AI because it doesn't know how to generate short lines.

---

_Comment by @MichaReiser on 2024-09-22 07:37_

I think this is similar to https://github.com/astral-sh/ruff/issues/7696 and I propose that we merge the issues. The main difference is that you propose the use of pragma comments.

Are all these files that need a longer line length in the same directory? If so, then you can add a new `ruff.toml` inside that directory, set it to `extend = <base_config_path>` your base configuration and change the `line-length`. Ruff then applies this configuration to all files in that directory (and its sub-directories)

Another alternative is to use `ruff format` which breaks the lines for you (or your AI ;)). 

---

_Closed by @zanieb on 2024-09-24 19:47_

---

_Label `duplicate` added by @zanieb on 2024-09-24 19:47_

---

_Comment by @toppk on 2024-09-27 09:35_


> Another alternative is to use `ruff format` which breaks the lines for you (or your AI ;)).

it's those f-strings that i'm always having to fix up.  ruff format does fine with the rest.


---

_Comment by @MichaReiser on 2024-09-27 09:39_

You can enable `ruff.format.preview` to get f-string formatting.

---
