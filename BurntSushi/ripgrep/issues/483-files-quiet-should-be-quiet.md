```yaml
number: 483
title: "--files --quiet should be quiet"
type: issue
state: closed
author: alecthomas
labels:
  - bug
assignees: []
created_at: 2017-05-17T05:02:17Z
updated_at: 2017-05-20T00:00:48Z
url: https://github.com/BurntSushi/ripgrep/issues/483
synced_at: 2026-01-12T16:13:22Z
```

# --files --quiet should be quiet

---

_@alecthomas_

`rg --files` always lists files, even if `--quiet` is passed. I did not expect that.

---

_Comment by @nateozem on 2017-05-17 10:39_

From `--help`

	   -q, --quiet
	           Do not print anything to stdout. If a match is found in a file, stop searching. This is
	           useful when ripgrep is used only for its exit code.

"anything" didn't apply, unless `--quiet` needs a redefinition.

---

_Comment by @BurntSushi on 2017-05-17 10:59_

I suppose in order to make this useful, ripgrep should return an error exit code if `--files` would otherwise result in no output. (Otherwise, I don't know what the point of `--quiet` is.)

---

_Label `question` added by @BurntSushi on 2017-05-17 10:59_

---

_Comment by @BurntSushi on 2017-05-17 10:59_

@alecthomas Can you say more about why you're trying to use `--files --quiet`?

---

_Comment by @alecthomas on 2017-05-17 11:01_

Similar to why I'd use a normal ripgrep(!)... I want to know if files with a certain extension exist, for the purposes of further processing. A bit like `find . -type f -name '*.h' > /dev/null && do_something`

---

_Comment by @alecthomas on 2017-05-17 11:02_

I'm doing `if rg --files --quiet --glob '*.go'; then ... fi`

---

_Comment by @BurntSushi on 2017-05-17 11:03_

Ah, OK, I actually didn't even know ripgrep already handled the exit code correctly here. Nice. Yes, `--quiet` should work.

---

_Label `bug` added by @BurntSushi on 2017-05-17 11:04_

---

_Label `question` removed by @BurntSushi on 2017-05-17 11:04_

---

_Comment by @alecthomas on 2017-05-17 13:49_

👍

---

_Closed by @BurntSushi on 2017-05-20 00:00_

---
