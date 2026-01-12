```yaml
number: 16008
title: "Should/make `uv cache prune` delete packages that are not used anywhere via link count"
type: issue
state: open
author: Tremeschin
labels:
  - enhancement
assignees: []
created_at: 2025-09-24T00:54:49Z
updated_at: 2025-09-25T10:25:05Z
url: https://github.com/astral-sh/uv/issues/16008
synced_at: 2026-01-12T16:02:21Z
```

# Should/make `uv cache prune` delete packages that are not used anywhere via link count

---

_@Tremeschin_

### Summary

I was wondering, since uv uses aggressive caching with hardlinks/symlinks ([related doc](https://docs.astral.sh/uv/reference/settings/#link-mode)), and that there's ways to count how many references a file has (at least for ext4 in linux, others may vary) with `stat -c "%h" (file)`, should the `uv cache prune` command remove cache entries which are only referenced once (ie. nowhere else except cache)?

It's better explained in examples, a bit lengthy as I've tried making it didactic. Perhaps I did something wrong and it already works this way - but I've experimented a bit with it and doesn't seem to be the case (could be based on last date of use?):

<sup><b>Note:</b> Didn't find a similar issue, as wording this is a bit hard.</sup>


### Example

For example, let's create two envs named `alpha` and `beta` after nuking `~/.cache` and `~/.local/share/uv`:

```sh
~/example/alpha
$ uv venv

~/example/beta
$ uv venv
```

Now, let's install a common package in both envs:

```sh
~/example/alpha
$ uv pip install cowsay==6.1

~/example/beta
$ uv pip install cowsay==6.1
```

We can see there's three hardlinks to the same file inode:

```sh
~/example/alpha
$ stat -c "%h" .venv/lib/python3.13/site-packages/cowsay/__init__.py
3

~/example/beta
$ stat -c "%h" .venv/lib/python3.13/site-packages/cowsay/__init__.py
3
```

As expected, `uv cache prune` doesn't remove anything:

```sh
$ uv cache prune
Pruning cache at: /home/tremeschin/.cache/uv
No unused entries found
```

Now, let's remove the `alpha` venv:

```sh
~/example/alpha
$ rm -rf .venv
```

Same thing, `uv cache prune` shouldn't and doesn't remove anything, count at two:

```sh
$ uv cache prune
Pruning cache at: /home/tremeschin/.cache/uv
No unused entries found

~/example/beta
$ stat -c "%h" .venv/lib/python3.13/site-packages/cowsay/__init__.py
2
```

But if we remove the `beta` venv as well:

```sh
~/example/beta
$ rm -rf .venv
```

Now, `uv cache prune` doesn't remove the "dangling" package entry:

```sh
$ uv cache prune
Pruning cache at: /home/tremeschin/.cache/uv
No unused entries found
```

Looking at `tree ~/.cache/uv` I see that the `cowsay` went to:

```sh
$ tree ~/.cache/uv/archive-v0
/home/tremeschin/.cache/uv/archive-v0/
└── 9HdHFxScxH75hluE_ypNq
    ├── cowsay
    │   ├── characters.py
    │   ├── __init__.py
```

Surely enough, the `__init__.py` has only one reference:

```sh
$ stat -c "%h" ~/.cache/uv/archive-v0/9HdHFxScxH75hluE_ypNq/cowsay/__init__.py
1
```

And if we install `cowsay` again in a venv, it has two:

```sh
~/example/alpha
$ uv venv
$ uv pip install cowsay
$ stat -c "%h" ~/.cache/uv/archive-v0/9HdHFxScxH75hluE_ypNq/cowsay/__init__.py
2
```

In my mind, `uv cache prune` should remove entries with only one reference - since that means it's not used anywhere else and can backlog space. Found this with a pytorch surprise in my home's cache directory, as `wheels-vX` or `archive-vX` doesn't change often and I assumed prune dealt with this already haha 🙂

Maybe there's better places to tell within uv cache when this happens, I'm not knowledgeable on the struct.

Thanks for the attention, sorry if duped, curious if it has ever been considered otherwise :)


---

_Label `enhancement` added by @Tremeschin on 2025-09-24 00:54_

---

_Comment by @konstin on 2025-09-25 10:25_

How to effectively limit the cache size is something we're still working on. While it's possible to use the link count, note the one advantage of hardlinks is that you can clear the cache without affecting any existing venv. For a cache garbage collection strategy, I'd lean more towards something like a least recently used cache, where we track when a package was last used in an installation, and clean everything that's older than a threshold or until we're under a target package size or count.

---
