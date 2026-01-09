---
number: 17619
title: Invalid .ruff_cache (deleting .ruff_cache produces different results)
type: issue
state: closed
author: PCSwingle
labels:
  - bug
  - cli
assignees: []
created_at: 2025-04-25T00:21:34Z
updated_at: 2025-11-21T23:39:26Z
url: https://github.com/astral-sh/ruff/issues/17619
synced_at: 2026-01-07T13:12:16-06:00
---

# Invalid .ruff_cache (deleting .ruff_cache produces different results)

---

_Issue opened by @PCSwingle on 2025-04-25 00:21_

### Summary

I stumbled upon a scenario where running `ruff format --check .` finds all files formatted (despite one file being unformatted), but deleting `.ruff_cache` and then rerunning the same command finds one file to reformat (in this case the file in question is using single quotes instead of double quotes). 

Unfortunately, I discovered this bug inside a docker container that I was using for a project, and it runs a lot of very gnarly scripts; long story short, I haven't been able to reproduce it yet without fully running through my project (after ~30 minutes of trying).

I'm willing to figure out an easier way to reproduce it (especially because I need ruff to work! although I could just run with no-cache), but I first wanted to check to make sure this is 100% a bug before I waste my time. Is there any scenario at all where running this direct sequence of commands:
```
ruff format --check .
rm -r .ruff_cache
ruff format --check .
```
should produce different results on the first and third command? (I haven't tested with `--no-cache` yet, but I can if that would be helpful). I have no relevant background processes or anything modifying files in the background. This is happening on a docker image based on `ubuntu:latest` on `ruff 0.11.6` and `python 3.12.3`.

### Version

ruff 0.11.6

---

_Comment by @MichaReiser on 2025-04-25 06:16_

Hi @PCSwingle 

No, running `ruff format --check` twice should produce the same results regardless of the precense of a cache. `ruff format --no-cache` should be equivalent to deleting the cache folder. At least, that would be the ideal state.

Ruff uses a file's `mtime` to determine if a file has changed. This is a somewhat inaccurate approach because mtime's can be rounded/trimmed, meaning changing a file twice in a very short period might result in the same `mtime`, which Ruff then fails to pick up. 

The only solution I'm aware of that isn't prone to this issue is to request (I remember someone mentioned that it's possible to ask git for file hashes) or compute the hash for every single file and compare them. However, this is significantely slower than just comparing mtimes because it requires reading every single file.

Rust has a similar problem and they now introduce a new nightly feature to enable hashing over mtimes https://doc.rust-lang.org/cargo/reference/unstable.html#checksum-freshness

---

_Label `bug` added by @MichaReiser on 2025-04-25 06:16_

---

_Label `cli` added by @MichaReiser on 2025-04-25 06:16_

---

_Comment by @PCSwingle on 2025-04-25 18:48_

Thank you, that makes a ton of sense! It turns out it's technically more of a problem on my end then. I was creating a tar archive and adding a file from scratch in python and sending it over to the docker container to modify the file, and python doesn't automatically set an mtime for the file in the tar archive. So the first time I modified the file, it set the mtime to epoch time 0, then I ran ruff and it saw the timestamp 0, then when I modified it again it kept the modified time as epoch time 0.

Closing this as it's more my problem than ruff's problem. Thank you!

---

_Closed by @PCSwingle on 2025-04-25 18:48_

---

_Comment by @vadimkantorov on 2025-11-21 23:39_

Also stumbled on this. Ruff was failing a file on GitHub CI, but not on my machine.

Cache was to blame.

It would be good to somehow improve UX around this. Maybe suggest to the user to also run with `--no-cache` when "all checks passed" gets printed

---
