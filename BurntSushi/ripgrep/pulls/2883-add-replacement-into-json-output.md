```yaml
number: 2883
title: Add replacement into json output
type: pull_request
state: closed
author: MagicDuck
labels:
  - rollup
assignees: []
base: master
head: add-replacement-text
created_at: 2024-09-08T09:24:00Z
updated_at: 2025-09-20T01:08:29Z
url: https://github.com/BurntSushi/ripgrep/pull/2883
synced_at: 2026-01-12T18:23:14Z
```

# Add replacement into json output

---

_@MagicDuck_

fixes https://github.com/BurntSushi/ripgrep/issues/1872

Hi, I am new to this repo so feel free to let me know if there is any way I can improve this PR. As detailed in the liked issue, this functionality is useful for quite a few cases where search/replace type tooling is built around ripgrep. My own neovim plugin is an example of a tool built on top of your great work 😄 : https://github.com/MagicDuck/grug-far.nvim 
This change would allow me to show diffs, a much requested feature.

Example:

Given the following text in file `/home/andrew/sherlock`:
```text
For the Doctor Watsons of this world, as opposed to the Sherlock
Holmeses, success in the province of detective work must always
```

Searching for `Watson` with a replacement parameter of `Moriarity`, here's what a match type item would looks like:
```json
{
  "type": "match",
  "data": {
    "path": {"text": "/home/andrew/sherlock"},
    "lines": {"text": "For the Doctor Watsons of this world, as opposed to the Sherlock\n"},
    "line_number": 1,
    "absolute_offset": 0,
    "submatches": [
      {"match": {"text": "Watson"}, "replacement": {"text": "Moriarity"}, "start": 15, "end": 21}
    ]
  }
}
```

P.S. Your documentation in code is amazing!



---

_Comment by @MagicDuck on 2024-09-08 09:30_

hmm, not sure why the musleabihf test failed, it just says `This job failed`...

---

_Comment by @MagicDuck on 2024-09-08 17:50_

hmm, passed now after changing a comment, just a blip I guess 😆 

---

_Label `rollup` added by @BurntSushi on 2025-07-26 15:36_

---

_Comment by @BurntSushi on 2025-07-26 15:36_

I've picked this up into my rollup branch. This was overall very well done. The only thing missing was an integration test checking that this worked (which I've added). Thank you for adding docs as well!

---

_Comment by @MagicDuck on 2025-07-28 17:16_

That's great news! Thank you so much! Looking forward to the rollup branch changes going out! 😄 

---

_Comment by @BurntSushi on 2025-07-28 17:26_

Yeah this PR will get closed automatically once the rollup branch gets merged.

It will still be a bit. I have a big backlog to work through.

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
