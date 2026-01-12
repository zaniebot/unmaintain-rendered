```yaml
number: 59
title: Missing benchmark for GNU grep
type: issue
state: closed
author: silversquirl
labels: []
assignees: []
created_at: 2016-09-24T11:13:37Z
updated_at: 2019-09-29T19:57:40Z
url: https://github.com/BurntSushi/ripgrep/issues/59
synced_at: 2026-01-12T16:13:21Z
```

# Missing benchmark for GNU grep

---

_@silversquirl_

ripgrep claims to be faster than GNU grep, however I see no benchmarks for GNU grep in the README. Before I can evaluate whether ripgrep would be useful to me, I would need to see some _significant_ improvements over `grep -r` in terms of speed.

`.gitignore` support is not particularly useful to me and grep can search only specific files with the help of globs (`grep pattern {,**/}*.py`), so speed would be the only real benefit over GNU grep and, personally, I doubt that ripgrep is fast enough to make a difference.


---

_Comment by @BurntSushi on 2016-09-24 12:24_

The README links to benchmarks: http://blog.burntsushi.net/ripgrep

On Sep 24, 2016 7:13 AM, "Samadi van Koten" notifications@github.com
wrote:

> ripgrep claims to be faster than GNU grep, however I see no benchmarks for
> GNU grep in the README. Before I can evaluate whether ripgrep would be
> useful to me, I would need to see some _significant_ improvements over grep
> -r in terms of speed.
> 
> .gitignore support is not particularly useful to me and grep can search
> only specific files with the help of globs (grep pattern {,*_/}_.py), so
> speed would be the only real benefit over GNU grep and, personally, I doubt
> that ripgrep is fast enough to make a difference.
> 
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/59, or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34umOiDl9pDKhPIF_o0F8FQwqUWENks5qtQXigaJpZM4KFnkm
> .


---

_Comment by @silversquirl on 2016-09-24 13:20_

Ah yes, I found the GNU grep benchmarks. Thanks!

Personally, I'd consider adding GNU grep to the list of benchmarks on the README, as it's mentioned frequently throughout other parts of said README.


---

_Closed by @silversquirl on 2016-09-24 13:20_

---

_Comment by @ghost on 2019-09-29 19:57_

`rg`8892bf648cfec111e6e7ddd9f30e932b0371db68  is like 4.5 times faster than gnu `grep`(3.3) when first run aka not disk-cached
and
`rg` is 2.8 times faster when disk-cached.

ran on chromium repo.
test based on https://github.com/BurntSushi/ripgrep/issues/1392#issuecomment-536330689

---
