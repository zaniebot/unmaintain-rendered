```yaml
number: 270
title: "`-e` flag doesn't work"
type: issue
state: closed
author: CamJN
labels: []
assignees: []
created_at: 2016-12-06T17:56:33Z
updated_at: 2016-12-07T15:32:01Z
url: https://github.com/BurntSushi/ripgrep/issues/270
synced_at: 2026-01-12T16:13:21Z
```

# `-e` flag doesn't work

---

_@CamJN_

The command `rg -F -e '-lselinux'` doesn't do the same thing as `rg -F -- '-lselinux'` despite the man page saying that `-e` should deal with the leading hyphen. `rg -F -e '-lselinux' --pretty` also doesn't work to override the `-l` that ripgrep thinks is there.

---

_Comment by @BurntSushi on 2016-12-06 18:03_

See the changelog. We had a regression here recently. Hopefully should be
fixed soon when I update clap. (On mobile.)

On Dec 6, 2016 12:56 PM, "Camden Narzt" <notifications@github.com> wrote:

> The command rg -F -e '-lselinux' doesn't do the same thing as rg -F --
> '-lselinux' despite the man page saying that -e should deal with the
> leading hyphen. rg -F -e '-lselinux' --pretty also doesn't work to
> override the -l that ripgrep thinks is there.
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/270>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34vJQKYTrhXkEdMnD5MwExA3fVgEZks5rFaHRgaJpZM4LFtCN>
> .
>


---

_Closed by @BurntSushi on 2016-12-07 15:32_

---
