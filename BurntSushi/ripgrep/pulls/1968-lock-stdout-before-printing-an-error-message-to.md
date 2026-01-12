```yaml
number: 1968
title: Lock stdout before printing an error message to stderr
type: pull_request
state: closed
author: film42
labels:
  - rollup
assignees: []
base: master
head: gt/lock_stderr
created_at: 2021-08-10T05:36:32Z
updated_at: 2023-07-10T17:26:59Z
url: https://github.com/BurntSushi/ripgrep/pull/1968
synced_at: 2026-01-12T18:23:14Z
```

# Lock stdout before printing an error message to stderr

---

_@film42_

This avoids interleaving error messages messages when search is writing to stdout.

Closes #1941

There are probably a few other places we'd want to make a similar patch.

---

_Comment by @BurntSushi on 2021-08-10 14:53_

> There are probably a few other places we'd want to make a similar patch.

I believe the only other place is in the logger? Maybe it would make sense to write our own `eprintln_locked` macro that does this, and then just replace every call to `eprintln!` in `crates/core` with that new macro.

I think this patch is right, but it's an unfortunate abstraction violation. This only works because the buffer printer in `search_parallel` will acquire the same stdout lock, which is done inside of `termcolor`. To avoid the abstraction violation, I think we'd need _another_ mutex in `crates/core` itself, but I'm not so sure it's worth it.

To mitigate the abstraction violation, could you make the comment added here mention and acknowledge the abstraction violation? Basically what I said above. And if we put this in its own macro, we only need to write this comment once.

---

_Comment by @film42 on 2021-08-10 19:19_

Adding a new macro certainly cleans things up a lot. I added a comment in the code (but not as part of the public documentation) since a comment about an abstraction violation may not be useful to the user outside of the code. Happy to make that part of the public docs though.

In my first cut, I added a lazy static rwlock and mapped the `read` to `STDOUT` with `write` mapping to `STDERR` to keep logging decently quick even with a mutex, but ultimately, locking `STDOUT` was a lot cleaner and intuitive. It also accomplishes the same thing.

---

_Label `rollup` added by @BurntSushi on 2023-07-08 21:41_

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---

_Branch deleted on 2023-07-10 17:26_

---
