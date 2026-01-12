```yaml
number: 1916
title: Allow to pass additional parameters do decompressor
type: issue
state: closed
author: marcin-github
labels:
  - wontfix
assignees: []
created_at: 2021-06-28T18:55:40Z
updated_at: 2021-06-30T13:31:22Z
url: https://github.com/BurntSushi/ripgrep/issues/1916
synced_at: 2026-01-12T16:13:24Z
```

# Allow to pass additional parameters do decompressor

---

_@marcin-github_

Hello!
I noticed I can't use ripgrep with files compressed using high compression levels of zstd.
```
$ rg -z "fooo" bigfile.json.zst
bigfile.json.zst:
-------------------------------------------------------------------------------
bigfile.json.zst : Decoding error (36) : Frame requires too much memory for decoding
bigfile.json.zst : Window size larger than maximum : 268435456 > 134217728
bigfile.json.zst : Use --long=28 or --memory=256MB
-------------------------------------------------------------------------------
``` 

Now I have to pipe file from `zstd -dc` to `rg` . It would be nice to have possibility to add "--long=foo" to decompresor. I can't do this using env variables because zstd doesn't support changing this setting via env.


---

_Comment by @BurntSushi on 2021-06-29 10:54_

@marcin-github Can you use the `--pre` flag (perhaps in concert with `--pre-glob`) to work-around this?

---

_Label `question` added by @BurntSushi on 2021-06-29 10:55_

---

_Comment by @marcin-github on 2021-06-30 13:11_

I read documentation about `--pre` option. It looks it can be workarround not convenient but makes rg usable:) Thanks.


---

_Comment by @BurntSushi on 2021-06-30 13:31_

All right, I'm going to close this then. The `-z/--search-zip` flag is more of a convenience option, and I'm not keen on adding more complexity to that. The `--pre` flag is really the right choice here, since it's one small entry point into ripgrep that permits a lot of flexibility over how to read data from disk. Consider, for example, what it would take to satisfy your feature request as stated. You can't just say, `--decompressor-args "--long=foo"` because there are multiple decompressors. So then do you do something like `--decompressor-zstd-args`? And create one such flag for each decompressor? Or perhaps create a mini-little DSL like how file types work, e.g., `--decompressor-args "zstd='--long=foo',gz='...'"` and so on. It's a UX nightmare.

Another possibility here is to just bump up the amount of memory we're wiling to let `zstd` use by default. I would potentially be open to a PR that made such a change, although I'd want to see some analysis on what the downsides are.

---

_Closed by @BurntSushi on 2021-06-30 13:31_

---

_Label `question` removed by @BurntSushi on 2021-06-30 13:31_

---

_Label `wontfix` added by @BurntSushi on 2021-06-30 13:31_

---
