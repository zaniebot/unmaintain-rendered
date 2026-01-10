---
number: 6335
title: No Python files found under the given path(s)
type: issue
state: closed
author: ghost
labels:
  - needs-decision
assignees: []
created_at: 2023-08-04T09:35:34Z
updated_at: 2023-08-05T19:45:51Z
url: https://github.com/astral-sh/ruff/issues/6335
synced_at: 2026-01-10T01:22:45Z
---

# No Python files found under the given path(s)

---

_Issue opened by @ghost on 2023-08-04 09:35_

Despite many Python files existing in subdirectories of the current directory, none are found.

```sh
$ ruff --version
ruff 0.0.282

$ ruff --isolated .
warning: No Python files found under the given path(s)

$ ls bin/
monophony.py

$ bat pyproject.toml
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ [tool.ruff]
   2   │ builtins = ['_']
   3   │ extend-ignore = ['E4', 'E74', 'E722']
   4   │ extend-select = ['ARG', 'Q']
   5   │ #tab_size = 4
   6   │
   7   │ [tool.ruff.flake8-quotes]
   8   │ inline-quotes = 'single'
   9   │ docstring-quotes = 'single'
  10   │ multiline-quotes = 'single'
───────┴───────────────────────────────────────────────────────────────────────────────────────────────────
```



---

_Comment by @charliermarsh on 2023-08-04 13:28_

Is `bin` in a `.gitignore`, perhaps? Can you run with `--verbose`?

---

_Label `waiting-on-author` added by @charliermarsh on 2023-08-04 13:28_

---

_Comment by @ghost on 2023-08-04 14:55_

> Is `bin` in a `.gitignore`, perhaps? Can you run with `--verbose`?

I got it. It's going all the way up to the `.gitignore` I have in my home directory and using that instead of the local one.

```sh
...
from: Some("/home/z/.gitignore"), original: "*"
...
```

To clarify, my home directory is a git repository, and all my projects have their own repositories inside it. None of the tools I use have any issues with this setup. Ruff also used to work fine until now.

---

_Comment by @charliermarsh on 2023-08-04 14:58_

Is there a `.gitignore` in the local directory too? Perhaps related to https://github.com/astral-sh/ruff/pull/5937, but not sure.

---

_Comment by @ghost on 2023-08-04 15:10_

> Is there a `.gitignore` in the local directory too?

Yes.


---

_Comment by @charliermarsh on 2023-08-05 15:06_

(This sounds like a bug, but I need to find time to reproduce locally before I can debug.)

---

_Label `waiting-on-author` removed by @charliermarsh on 2023-08-05 15:07_

---

_Label `needs-decision` added by @charliermarsh on 2023-08-05 15:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-05 15:07_

---

_Comment by @charliermarsh on 2023-08-05 16:31_

I was able to reproduce! It looks like this is identical to https://github.com/BurntSushi/ripgrep/issues/2447. FWIW the workarounds mentioned in that issue did work for me.

@BurntSushi -- do you know if there's any way to avoid traversing past the "current" Git repository when enforcing `.gitignore` behavior in the `ignore` crate? (If not, I'm gonna close and suggest those workarounds as it's out of our control.)

---

_Comment by @BurntSushi on 2023-08-05 16:52_

The specific case of nested git repositories should be handled correctly. Or at least, the [`ignore` crate attempts to do so](https://github.com/BurntSushi/ripgrep/blob/61733f6378b62fa2dc2e7f3eff2f2e7182069ca9/crates/ignore/src/dir.rs#L461-L473). Perhaps there is a bug there. A specific repro would be helpful in that case.

But this doesn't apply to `.ignore` or `.rgignore` files, only `.gitignore`. For `.ignore` and `.rgignore`, there is no way to avoid traversing a certain point.

---

_Comment by @charliermarsh on 2023-08-05 16:57_

Okay cool, thanks. I'll put together a specific repro (both to confirm and facilitate debugging).

---

_Comment by @charliermarsh on 2023-08-05 17:14_

If I clone [this](https://github.com/charliermarsh/ignore_crate_repro), then run `cargo run`:

```console
❯ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/ignore_crate_repro`
./python
./python/baz.py
./python/bar.py
```

However, if I add a `.gitignore` to the parent directory, which is a Git repo, with:

```text
baz.py
```

And then run `cargo run`:

```console
❯ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/ignore_crate_repro`
./python
./python/bar.py
```

---

_Comment by @charliermarsh on 2023-08-05 17:15_

Alternatively, if the parent directory _isn't_ a Git repo, you can set apply this to get the same behavior:

```diff
diff --git a/src/main.rs b/src/main.rs
index 012362f..5e4dbe4 100644
--- a/src/main.rs
+++ b/src/main.rs
@@ -1,7 +1,7 @@
 use ignore::WalkBuilder;

 fn main() {
-    for result in WalkBuilder::new("./python").build() {
+    for result in WalkBuilder::new("./python").require_git(false).build() {
         match result {
             Ok(entry) => println!("{}", entry.path().display()),
             Err(err) => println!("ERROR: {}", err),
```

---

_Comment by @BurntSushi on 2023-08-05 18:07_

Interestingly, I can't reproduce it:

```
$ cd /tmp
$ git clone https://github.com/charliermarsh/ignore_crate_repro
$ cd ignore_crate_repro
$ cargo run
./python
./python/baz.py
./python/bar.py
$ echo baz.py > /tmp/.gitignore
$ cat /tmp/.gitignore
baz.py
$ cargo run
./python
./python/baz.py
./python/bar.py
```

And note that ripgrep handles this correctly too:

```
$ rg --files
Cargo.lock
Cargo.toml
LICENSE
pyproject.toml
python/bar.py
python/baz.py
src/main.rs
```

And if I change the parent `.gitignore` to a `.ignore`, then it is respected (as expected):

```
$ mv /tmp/.gitignore /tmp/.ignore
$ cargo run
./python
./python/bar.py
```

Or wait, okay, I missed that you said the parent directory is also a git repo. `/tmp` is not a git repo, so let's try that:

```
$ cd /tmp
$ rm -rf ignore_crate_repro/
$ rm .ignore
$ mkdir parentrepo && cd parentrepo
$ git init
Initialized empty Git repository in /tmp/parentrepo/.git/
$ touch dummy
$ git add dummy && git commit -m "dummy commit"
$ git clone https://github.com/charliermarsh/ignore_crate_repro
$ cd ignore_crate_repro
$ cargo run
./python
./python/baz.py
./python/bar.py
$ echo baz.py > ../.gitignore
$ cat ../.gitignore
baz.py
$ cargo run
./python
./python/baz.py
./python/bar.py
$ rg --files
Cargo.lock
Cargo.toml
LICENSE
pyproject.toml
python/bar.py
python/baz.py
src/main.rs
```

So all still seems well to me? I wouldn't expect the parent directory being a git repo to matter here because the parent traversal logic should stop right after it sees a git repo, which in this case is the current working directory.

And yeah, when `require_git` is disabled, then `ignore` specifically doesn't care about whether a git repo exists or not. It just blindly applies `.gitignore` filtering.

---

_Comment by @charliermarsh on 2023-08-05 19:12_

Ahh okay, I see. So this is partially my fault, apologies. Without `require_git(false)`, it turns out I'm only seeing this when `.gitignore` is in my root directory, and I noticed that I have this in my `~/.gitconfig`:

```
[user]
    name = Charlie Marsh
    email = charlie.r.marsh@gmail.com
[core]
    excludesfile = ~/.gitignore
```

Removing the `excludesfile = ~/.gitignore` fixes the issue for me (when _not_ adding `require_git(false)`).

So I'm guessing that this behavior in Ruff stems from us setting `require_git(false)`, which was done to fix https://github.com/astral-sh/ruff/issues/5930. I may just have to revert that, if this is the expected behavior (which it sounds like it is).

---

_Comment by @BurntSushi on 2023-08-05 19:21_

Yeah it sounds like expected behavior to me. Note that ripgrep exposes the `require_git` option as a `--no-require-git` flag. Some people really want ripgrep to not care about `.git` and some do. IIRC, ripgrep originally didn't care, but the VS Code folks convinced me to switch over to caring about whether to respect `.gitignore` based on whether `.git` was there or not. (I may be mis-remembering, this was a while ago.)

---

_Referenced in [astral-sh/ruff#6368](../../astral-sh/ruff/pulls/6368.md) on 2023-08-05 19:35_

---

_Referenced in [astral-sh/ruff#5930](../../astral-sh/ruff/issues/5930.md) on 2023-08-05 19:38_

---

_Closed by @charliermarsh on 2023-08-05 19:45_

---
