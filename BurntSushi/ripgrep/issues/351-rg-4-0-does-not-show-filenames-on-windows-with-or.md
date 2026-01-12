```yaml
number: 351
title: rg 4.0 does not show filenames on Windows, with or without --with-filename
type: issue
state: closed
author: charlieflowers
labels:
  - question
assignees: []
created_at: 2017-02-07T01:34:39Z
updated_at: 2017-06-28T14:04:25Z
url: https://github.com/BurntSushi/ripgrep/issues/351
synced_at: 2026-01-12T16:13:21Z
```

# rg 4.0 does not show filenames on Windows, with or without --with-filename

---

_@charlieflowers_

If you search multiple files without --with-filename, or if you explicitly specify --with-filename, you still get results without filenames. I fell back to 3.2 which resolved the issue for me.

---

_Comment by @BurntSushi on 2017-02-07 01:44_

Could you please show the exact commands you are running and their output?

On Feb 6, 2017 8:34 PM, "Charlie Flowers" <notifications@github.com> wrote:

> If you search multiple files without --with-filename, or if you explicitly
> specify --with-filename, you still get results without filenames. I fell
> back to 3.2 which resolved the issue for me.
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/351>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34gswGlTRew7cSzJyDgqcjB-bP9Csks5rZ8ovgaJpZM4L48Zw>
> .
>


---

_Label `question` added by @BurntSushi on 2017-02-18 17:40_

---

_Comment by @BurntSushi on 2017-02-18 17:40_

Closing due to inactivity. I can't reproduce this.

---

_Closed by @BurntSushi on 2017-02-18 17:40_

---

_Comment by @otterpro on 2017-06-27 21:46_

This happens in Powershell and filenames don't get displayed no matter what.  However, it works as expected in CMD.

---

_Comment by @BurntSushi on 2017-06-27 21:51_

@otterpro Please show the exact command you're running and the output.

---

_Comment by @otterpro on 2017-06-28 13:57_

Ah, I tried it without  color (`--color never`) in Powershell, and it works fine. 

Filenames are hidden when:
rg "hello" -g "*.cs"  # cannot see filename

Filenames are  visible when:
rg "hello" -g "*.cs" --color never  
# OR
rg "hello" -g "*.cs" --vimgrep
 
![2017-06-28 09_52_21-windows powershell](https://user-images.githubusercontent.com/54618/27640747-13e3c390-5be8-11e7-8c62-48d52ce7a951.png)
![2017-06-28 09_54_02-windows powershell](https://user-images.githubusercontent.com/54618/27640748-13e86936-5be8-11e7-97e6-8be4862c3f49.png)



---

_Comment by @BurntSushi on 2017-06-28 13:58_

@otterpro So it sounds like you need to change your color scheme then? e.g., `rg hello --colors 'path:fg:blue'` (or whatever).

---

_Comment by @otterpro on 2017-06-28 14:04_

Yes, apparently the default background color  (blue background) of Powershell hides the filenames.  I guess I'll change the background color of the console.  Thank you for your help.

---
