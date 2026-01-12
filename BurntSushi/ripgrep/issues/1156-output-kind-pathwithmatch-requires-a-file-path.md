```yaml
number: 1156
title: output kind PathWithMatch requires a file path
type: issue
state: closed
author: need47
labels: []
assignees: []
created_at: 2019-01-09T15:46:12Z
updated_at: 2019-01-09T16:35:59Z
url: https://github.com/BurntSushi/ripgrep/issues/1156
synced_at: 2026-01-12T16:13:23Z
```

# output kind PathWithMatch requires a file path

---

_@need47_

#### What version of ripgrep are you using?

Replace this text with the output of `rg --version`.
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

use Github binary release

#### What operating system are you using ripgrep on?

x86_64 GNU/Linux

#### Bug

I have a file named 'list', which contains two lines:
abc
xyz

Then I tested with the command below to show only the filename with match.

$ rg -l abc list
list: output kind PathWithMatch requires a file path

I didn't expect to see 'output kind PathWithMatch requires a file path'

Then I tested with multiple files, it looks good.

$ rg -l abc list list
list
list


---

_Comment by @BurntSushi on 2019-01-09 16:02_

This is a duplicate. I'm on mobile so I don't have a link, but it should
come up in a search.

On Wed, Jan 9, 2019, 10:46 need47 <notifications@github.com wrote:

> What version of ripgrep are you using?
>
> Replace this text with the output of rg --version.
> ripgrep 0.10.0 (rev 8a7db1a
> <https://github.com/BurntSushi/ripgrep/commit/8a7db1a918e969b85cd933d8ed9fa5285b281ba4>
> )
> -SIMD -AVX (compiled)
> +SIMD -AVX (runtime)
> How did you install ripgrep?
>
> use Github binary release
> What operating system are you using ripgrep on?
>
> x86_64 GNU/Linux
> Bug
>
> I have a file named 'list', which contains two lines:
> abc
> xyz
>
> Then I tested with the command below to show only the filename with match.
>
> $ rg -l abc list
> list: output kind PathWithMatch requires a file path
>
> I didn't expect to see 'output kind PathWithMatch requires a file path'
>
> Then I tested with multiple files, it looks good.
>
> $ rg -l abc list list
> list
> list
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1156>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34hSbGla-s9p0iaBIw0RGBnGQaptpks5vBg7FgaJpZM4Z3v9O>
> .
>


---

_Comment by @need47 on 2019-01-09 16:35_

Got it. #1130.

---

_Closed by @need47 on 2019-01-09 16:35_

---
