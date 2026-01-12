```yaml
number: 2566
title: "feat: dont do needless search when we reach max depth"
type: pull_request
state: closed
author: sigmaSd
labels: []
assignees: []
base: master
head: leaf
created_at: 2023-07-23T13:21:31Z
updated_at: 2023-09-18T15:53:36Z
url: https://github.com/BurntSushi/ripgrep/pull/2566
synced_at: 2026-01-12T18:23:14Z
```

# feat: dont do needless search when we reach max depth

---

_@sigmaSd_

fix https://github.com/BurntSushi/ripgrep/issues/2565

I can improve the code, if in general it seems reasonable,
seems to work in my testing

---

_Comment by @sigmaSd on 2023-07-23 13:58_

Btw this gets it down to 3 syscalls per entry  when max depth is reached
```
openat(AT_FDCWD, "/home/mrcool/dev/rust/nanoserde", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 4
newfstatat(4, "", {st_mode=S_IFDIR|0755, st_size=4096, ...}, AT_EMPTY_PATH) = 0
close(4)                                = 0
openat(AT_FDCWD, "/home/mrcool/dev/rust/message-io", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 4
newfstatat(4, "", {st_mode=S_IFDIR|0755, st_size=4096, ...}, AT_EMPTY_PATH) = 0
close(4)                                = 0
openat(AT_FDCWD, "/home/mrcool/dev/rust/mun_example", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 4
newfstatat(4, "", {st_mode=S_IFDIR|0755, st_size=4096, ...}, AT_EMPTY_PATH) = 0
close(4)                                = 0
openat(AT_FDCWD, "/home/mrcool/dev/rust/progress", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 4
newfstatat(4, "", {st_mode=S_IFDIR|0755, st_size=4096, ...}, AT_EMPTY_PATH) = 0
```
But I imagine even those are not needed, but I guess its probably negligible

---

_Comment by @BurntSushi on 2023-09-18 15:53_

Thank you, but I'm going to pass on this for now. I hope to do an overhaul of the `ignore` crate soon, and I'll see if it makes sense to take max depth into account at a more fundamental level. I'm not quite comfortable with this change as-is because I think it's a little too hacky. (Which isn't your fault. The crate's internals are an awful mess and I barely understand it any more.)

---

_Closed by @BurntSushi on 2023-09-18 15:53_

---
