```yaml
number: 532
title: No zsh filename tab-autocompletion
type: issue
state: closed
author: mattsmithdatera
labels:
  - help wanted
  - question
assignees: []
created_at: 2017-06-29T19:08:25Z
updated_at: 2017-07-03T11:17:24Z
url: https://github.com/BurntSushi/ripgrep/issues/532
synced_at: 2026-01-12T16:13:22Z
```

# No zsh filename tab-autocompletion

---

_@mattsmithdatera_

Info:
```zsh
rg --version
ripgrep 0.5.2

zsh --version
zsh 5.2 (x86_64-apple-darwin15.0.0)
```
Reproduction steps:
1. Use zsh shell
2. Copy _rg autocompletion file to zsh /usr/share/zsh/site-functions location
3. type `rg "test-match" /usr/
4. Start pressing `tab`.
5. Nothing is suggested

Expected behavior (demonstrated with bash shell, where this functions correctly):
1. Use bash shell
2. copy rg bash autocompletion file to /usr/local/etc/bash_completion.d
3. type `rg "test-match" /usr/`
4. Start pressing `tab`
5. Files in /usr/ are suggested for autocompletion

Additional info:
- Zsh autocompletes ripgrep options without issue
- Zsh fails to autocomplete files regardless of presence of zsh autocompletion file in site-functions folder
- ripgrep installed via Homebrew on OSX

---

_Comment by @BurntSushi on 2017-06-29 20:56_

Someone else will need to handle this.

---

_Label `help wanted` added by @BurntSushi on 2017-06-29 20:57_

---

_Label `question` added by @BurntSushi on 2017-06-29 20:57_

---

_Comment by @BurntSushi on 2017-06-29 20:57_

(I'm really not happy about ripgrep providing auto completions like this. I don't have the inclination to debug their problems.)

---

_Comment by @okdana on 2017-07-01 06:26_

The zsh completion function in the Homebrew package is not the one that's in the `ripgrep` repository. Not sure where theirs comes from, there's no attribution or anything. The one in the repo seems to work fine for me, at least in terms of completing the file name. There are some other (trivial) issues with it, though, that i can put together a PR for if you want. (The biggest one is that the descriptions for `--count` and `--max-count` are the same, which causes zsh to treat them as aliases for each other.)

ETA: Oh, their completion function must be the one that `clap` was auto-generating. It wasn't replaced by the hand-written one until a few weeks after 0.5.2 was released. Homebrew will probably pull in the new one whenever 0.5.3 lands.

---

_Comment by @BurntSushi on 2017-07-01 13:04_

@okdana PRs would be most appreciated. :-)

---

_Comment by @BurntSushi on 2017-07-03 11:16_

It sounds like this has been fixed, but you need to use the manually curated zsh completion script here: https://github.com/BurntSushi/ripgrep/blob/master/complete/_rg (Which should be the one that is bundled in the next release tarball.)

---

_Closed by @BurntSushi on 2017-07-03 11:17_

---
