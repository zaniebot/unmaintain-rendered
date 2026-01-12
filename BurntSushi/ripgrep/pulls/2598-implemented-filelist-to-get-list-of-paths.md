```yaml
number: 2598
title: implemented filelist to get list of paths
type: pull_request
state: closed
author: eyalk11
labels: []
assignees: []
draft: true
base: master
head: filelist
created_at: 2023-08-25T23:55:10Z
updated_at: 2023-09-18T15:54:22Z
url: https://github.com/BurntSushi/ripgrep/pull/2598
synced_at: 2026-01-12T18:23:14Z
```

# implemented filelist to get list of paths

---

_@eyalk11_

Added option to get list of paths. I got into trouble trying to work with list of files in neovim (leaderf or other), when the list of files I wanted to search over was very long.  I believe it solves https://github.com/BurntSushi/ripgrep/issues/273 . 

See also https://vi.stackexchange.com/questions/42785/how-to-do-fuzzy-live-grep-on-git-files/42915#42915

---

_@BurntSushi requested changes on 2023-08-29 03:03_



Thank you, but there are numerous issues with this PR as-is. The flag doesn't follow ripgrep's naming convention. Errors are squashed. The documentation for the flag is very insufficient. There are no tests. This also won't handle the case of file paths that contain invalid UTF-8. And perhaps more that I haven't noticed yet.

If you want to try to fix these things that would be great, but this is likely something I'll get to eventually myself if not.


---

_Comment by @eyalk11 on 2023-08-30 23:25_

Thanks for the comment. I appreciate it. 

It was quick-and-a-bit-dirty patch . I don't really know rust (chatgpt wrote half of it) . 
I see what I can do. I can improve documentation and naming, but won't add tests. As it is just too much giving complete lack of knowledge in language, and I am busy as it is. 

I feel like in any case it would be better to leave it up to you. But you can let me have a week and will try to get to it.


---

_Converted to draft by @eyalk11 on 2023-08-30 23:31_

---

_Comment by @BurntSushi on 2023-09-18 15:54_

OK, I'm going to close this out. I think this PR is pretty far from what this should ideally look like. I will very likely tackle this myself at some point.

---

_Closed by @BurntSushi on 2023-09-18 15:54_

---
