```yaml
number: 16617
title: Caching wheel files - does not refresh?
type: issue
state: open
author: behrenhoff
labels:
  - bug
assignees: []
created_at: 2025-11-06T17:49:31Z
updated_at: 2025-11-08T19:49:38Z
url: https://github.com/astral-sh/uv/issues/16617
synced_at: 2026-01-12T16:02:35Z
```

# Caching wheel files - does not refresh?

---

_@behrenhoff_

### Summary

I am creating a wheel file out of my lib.
When testing the wheel, I use it outside of my repository - and to do that, I use `uv run --isolated --refresh --with  path/to/MYWHEEL.whl ...` 

That works **once** - but when I discover a bug and change the code + rebuild the wheel, uv keeps using the old wheel until I run `uv cache prune`.

According to the documentation, wheel files are cached via their modification time (source: https://docs.astral.sh/uv/concepts/cache/):

> For local dependencies, uv caches based on the last-modified time of the source archive (i.e., the local .whl or .tar.gz file).  

As my new wheel has a newer mtime, my expectation was that it would be picked up.

Is there any way uv could detect this by itself? Can I exclude certain packages from caching? This is in particular the case for local wheels which are being tested. Of course I usually want the dependencies of my wheel to come from cache (it might depend on huge packages like tensorflow, therefore disabling cache completely is not an option). 

A simple reproducer is this script (run it in a temporary directory):

```bash
#!/bin/bash

set -e

uv --version

rm -rf bug
uv cache prune

uv init --lib bug

cd bug

uv build --wheel
stat dist/bug-0.1.0-py3-none-any.whl
cd ..

echo
echo "First call"
echo "-----------------------------------------------"
uv run --isolated --refresh --with bug/dist/bug-0.1.0-py3-none-any.whl python -c 'import bug; print(bug.hello())'
echo "-----------------------------------------------"
echo

cd bug
cat << EOF > src/bug/__init__.py
def hello() -> str:
    return "Updated code!"
EOF
rm -f dist/bug-0.1.0-py3-none-any.whl  # does not matter, but this ensures all times are different
uv build --wheel
stat dist/bug-0.1.0-py3-none-any.whl
cd ..

echo
echo "Trying to run modified wheel (no output from uv"
echo "about reinstallation, the printout is still the"
echo "old one."
echo "-----------------------------------------------"
uv run --isolated --refresh --with bug/dist/bug-0.1.0-py3-none-any.whl python -c 'import bug; print(bug.hello())'
echo "-----------------------------------------------"
echo

uv cache prune
echo
echo "Running modified wheel after cache pruning"
echo "Now the output is correct"
echo "-----------------------------------------------"
uv run --isolated --refresh --with bug/dist/bug-0.1.0-py3-none-any.whl python -c 'import bug; print(bug.hello())'
echo "-----------------------------------------------"
```


The output is:
```
$ ./uv-chache-reproducer.sh
uv 0.9.7
Pruning cache at: /home/username/.cache/uv
Removed 35 files (31.9KiB)
Initialized project `bug` at `/tmp/bug`
Building wheel (uv build backend)...
Successfully built dist/bug-0.1.0-py3-none-any.whl
  File: dist/bug-0.1.0-py3-none-any.whl
  Size: 1364            Blocks: 8          IO Block: 4096   regular file
Device: 0,37    Inode: 6099        Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-11-06 18:41:21.143532545 +0100
Modify: 2025-11-06 18:41:21.144532555 +0100
Change: 2025-11-06 18:41:21.144532555 +0100
 Birth: 2025-11-06 18:41:21.143532545 +0100

First call
-----------------------------------------------
Installed 1 package in 0.31ms
Hello from bug!
-----------------------------------------------

Building wheel (uv build backend)...
Successfully built dist/bug-0.1.0-py3-none-any.whl
  File: dist/bug-0.1.0-py3-none-any.whl
  Size: 1363            Blocks: 8          IO Block: 4096   regular file
Device: 0,37    Inode: 6100        Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-11-06 18:41:21.308534251 +0100
Modify: 2025-11-06 18:41:21.309534261 +0100
Change: 2025-11-06 18:41:21.309534261 +0100
 Birth: 2025-11-06 18:41:21.308534251 +0100

Trying to run modified wheel (no output from uv
about reinstallation, the printout is still the
old one.
-----------------------------------------------
Hello from bug!
-----------------------------------------------

Pruning cache at: /home/username/.cache/uv
Removed 35 files (31.9KiB)

Running modified wheel after cache pruning
Now the output is correct
-----------------------------------------------
Installed 1 package in 0.30ms
Updated code!
-----------------------------------------------
```

As you can see, I get "Hello from bug!" twice, even after rebuilding the wheel. Only after pruning, the new wheel is used.


### Platform

Linux 6.14.0-35-generic x86_64 GNU/Linux

### Version

uv 0.9.7

### Python version

Python 3.12.3

---

_Label `bug` added by @behrenhoff on 2025-11-06 17:49_

---

_Comment by @zanieb on 2025-11-06 17:50_

cc @konstin 

---

_Comment by @charliermarsh on 2025-11-06 17:55_

My guess is that this is specific to `--with` where we use the cached environment concept.

---

_Comment by @caschb on 2025-11-08 19:49_

As @charliermarsh said, the origin of this problem is in the `CachedEnvironment`.  

https://github.com/astral-sh/uv/blob/1b7faafd7a64f5eb91743213ce14cbb8e3cd248f/crates/uv/src/commands/project/environment.rs#L151-L181  

The cache only uses the resolution and the interpreter path to create the content-addressed location, so if neither the resolution or the interpreter change, a collision happens.  

A way to solve this could be to include more information in the hash that is tied to the state of the project, but I'm not sure how to implement this, or if this is indeed the best way to solve this problem.  

> There are only two hard things in Computer Science: cache invalidation, naming things, and off by one errors
> - Phil Karlton

---
