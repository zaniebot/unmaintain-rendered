```yaml
number: 9492
title: "uv tool install --force doesn't properly sync with local modifications"
type: issue
state: open
author: CoderJoshDK
labels: []
assignees: []
created_at: 2024-11-28T03:02:36Z
updated_at: 2024-11-28T12:01:07Z
url: https://github.com/astral-sh/uv/issues/9492
synced_at: 2026-01-12T15:59:51Z
```

# uv tool install --force doesn't properly sync with local modifications

---

_@CoderJoshDK_

Using the command `uv tool install --force .`, doesn't actually update tool with new changes.

```sh
$ uv init fail --package && cd fail && uv tool install . --force
$ fail
Hello from fail!
```

From here, we will modify the print statement. 
```py
# src/fail/__init__.py
def main() -> None:
    print("Hello from fail! Modification!")
```

And now we re-force install and run:
```sh
$ uv tool install . --force
Resolved 1 package in 0.85ms
Uninstalled 1 package in 0.25ms
Installed 1 package in 1ms
 ~ fail==0.1.0 (from file:///home/josh/fail)
Installed 1 executable: fail
$ fail
Hello from fail!
$ uv run fail
Hello from fail! Modification!
```
As you can see, if I run `uv run fail`, it does output the expected updates. 

<details><summary>Verbose install</summary>
<p>

```sh
$ uv tool install . --force --verbose
DEBUG uv 0.5.5
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/home/josh/.local/share/uv/python`
DEBUG Found `cpython-3.9.19-linux-aarch64-gnu` at `/usr/bin/python3` (search path)
DEBUG Using request timeout of 30s
DEBUG Found PEP 621 metadata for /home/josh/fail in `pyproject.toml` (fail)
DEBUG Acquired lock for `/home/josh/.local/share/uv/tools`
DEBUG Found existing environment for tool `fail`: /home/josh/.local/share/uv/tools/fail
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: fail @ file:///home/josh/fail
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.9.19
DEBUG Solving with target Python version: >=3.9.19
DEBUG Adding direct dependency: fail*
DEBUG Searching for a compatible version of fail @ file:///home/josh/fail (*)
DEBUG Tried 1 versions: fail 1
DEBUG marker environment resolution took 0.000s
Resolved 1 package in 0.96ms
DEBUG Directory source requirement already cached: fail==0.1.0 (from file:///home/josh/fail)
DEBUG Uninstalled fail (10 files, 3 directories)
Uninstalled 1 package in 0.52ms
Installed 1 package in 3ms
 ~ fail==0.1.0 (from file:///home/josh/fail)
DEBUG Removing executable: `/home/josh/.local/bin/fail`
DEBUG Installing tool executables into: /home/josh/.local/bin
DEBUG Looking at `.dist-info` at: /home/josh/.local/share/uv/tools/fail/lib/python3.9/site-packages/fail-0.1.0.dist-info
DEBUG Installing executable: `fail`
Installed 1 executable: fail
DEBUG Adding receipt for tool `fail`
DEBUG Adding metadata entry for tool `fail` at /home/josh/.local/share/uv/tools/fail/uv-receipt.toml
DEBUG Released lock at `/home/josh/.local/share/uv/tools/.lock`
```

</p>
</details> 

We can actually see that the bin is being updated! But for some reason ,,, it isn't.

<details><summary>File modifications</summary>
<p>

```bash
$ fail
Hello from fail!
[josh@uv fail]$ ls -al ~/.local/share/uv/tools/fail/bin/
total 56
drwxr-xr-x 1 josh josh  266 Nov 27 21:54 .
drwxr-xr-x 1 josh josh  116 Nov 27 21:51 ..
-rw-r--r-- 1 josh josh 3713 Nov 27 21:51 activate
-rw-r--r-- 1 josh josh 2269 Nov 27 21:51 activate.bat
-rw-r--r-- 1 josh josh 2620 Nov 27 21:51 activate.csh
-rw-r--r-- 1 josh josh 4184 Nov 27 21:51 activate.fish
-rw-r--r-- 1 josh josh 3869 Nov 27 21:51 activate.nu
-rw-r--r-- 1 josh josh 2762 Nov 27 21:51 activate.ps1
-rw-r--r-- 1 josh josh 2415 Nov 27 21:51 activate_this.py
-rw-r--r-- 1 josh josh 1728 Nov 27 21:51 deactivate.bat
-rwxr-xr-x 1 josh josh  236 Nov 27 21:54 fail
-rw-r--r-- 1 josh josh 1215 Nov 27 21:51 pydoc.bat
lrwxrwxrwx 1 josh josh   16 Nov 27 21:51 python -> /usr/bin/python3
lrwxrwxrwx 1 josh josh    6 Nov 27 21:51 python3 -> python
lrwxrwxrwx 1 josh josh    6 Nov 27 21:51 python3.9 -> python
[josh@uv fail]$ uv tool install . --force
Resolved 1 package in 1ms
Uninstalled 1 package in 0.57ms
Installed 1 package in 4ms
 ~ fail==0.1.0 (from file:///home/josh/fail)
Installed 1 executable: fail
[josh@uv fail]$ ls -al ~/.local/share/uv/tools/fail/bin/
total 56
drwxr-xr-x 1 josh josh  266 Nov 27 21:56 .
drwxr-xr-x 1 josh josh  116 Nov 27 21:51 ..
-rw-r--r-- 1 josh josh 3713 Nov 27 21:51 activate
-rw-r--r-- 1 josh josh 2269 Nov 27 21:51 activate.bat
-rw-r--r-- 1 josh josh 2620 Nov 27 21:51 activate.csh
-rw-r--r-- 1 josh josh 4184 Nov 27 21:51 activate.fish
-rw-r--r-- 1 josh josh 3869 Nov 27 21:51 activate.nu
-rw-r--r-- 1 josh josh 2762 Nov 27 21:51 activate.ps1
-rw-r--r-- 1 josh josh 2415 Nov 27 21:51 activate_this.py
-rw-r--r-- 1 josh josh 1728 Nov 27 21:51 deactivate.bat
-rwxr-xr-x 1 josh josh  236 Nov 27 21:56 fail
-rw-r--r-- 1 josh josh 1215 Nov 27 21:51 pydoc.bat
lrwxrwxrwx 1 josh josh   16 Nov 27 21:51 python -> /usr/bin/python3
lrwxrwxrwx 1 josh josh    6 Nov 27 21:51 python3 -> python
lrwxrwxrwx 1 josh josh    6 Nov 27 21:51 python3.9 -> python
[josh@uv fail]$ fail
Hello from fail!
[josh@uv fail]$ uv run fail
Hello from fail! Modification!
```

</p>
</details> 

<details><summary>Same as above but less noise</summary>
<p>

```sh
$ fail
Hello from fail!

$ ls -al ~/.local/share/uv/tools/fail/bin/
drwxr-xr-x 1 josh josh  266 Nov 27 21:54 .
-rwxr-xr-x 1 josh josh  236 Nov 27 21:54 fail

$ uv tool install . --force
 ~ fail==0.1.0 (from file:///home/josh/fail)
Installed 1 executable: fail

$ ls -al ~/.local/share/uv/tools/fail/bin/
drwxr-xr-x 1 josh josh  266 Nov 27 21:56 .
-rwxr-xr-x 1 josh josh  236 Nov 27 21:56 fail

$ fail
Hello from fail!

$ uv run fail
Hello from fail! Modification!
```

</p>
</details> 

So everything is giving the appearance of updating, and yet it isn't.

Oh ,,, and sorry if the name of the package sounds passive aggressive. I only realized that it might sound that way after I wrote all of this ... I actually am super appreciative of all the work you guys have done! Thank you again!

---

_Comment by @powercoconola on 2024-11-28 04:42_

Just curious, does this work?
```
uv tool install . --reinstall
```

>Reinstall all packages, regardless of whether they’re already installed. Implies --refresh


---

_Comment by @powercoconola on 2024-11-28 04:51_

From the documentation it seems `--force` doesn't do what you think it does. 

>Will replace any existing entry points with the same name in the executable directory. 

I think it's just replacing the entrypoint (in the `/bin` directory) but not the `.py` files.

Additionally, your file structure seems a bit odd to me as well. I think the `/bin` directory is not supposed to be in the same directory where the tool `Scripts` and `Lib` are installed. I'm not familiar with Linux and `uv` though.

---

_Comment by @pnasrat on 2024-11-28 11:44_

According to this PR force should imply reinstall package but as the reproducer here shows its not working as intended 

https://github.com/astral-sh/uv/commit/b642dd92875456a175fe09d912a918d99db2ae27

Looking at the log output while the settings are being populated the difference I see with reinstall is rather than 

I think that if force is set similar to `target.is_latest` it probably want to set the cache with refresh around line 93 

```rust
let cache  = if force | target.is_latest() {
```

---

_Comment by @CoderJoshDK on 2024-11-28 12:01_

> From the documentation it seems --force doesn't do what you think it does.

`--force` was recently updated (the commit linked above). But it seems this change in behavior should be reflected in the docs too. 

> Additionally, your file structure seems a bit odd to me as well.

`uv tool` interacts with 2 locations. There is the `bin` and then the `tool` dir, where the bin just symlinks to the tool's bin. `--force` updates both that link and reinstalls the package:

```sh
$ ls -l ~/.local/bin/fail
lrwxrwxrwx 1 josh josh 46 Nov 28 06:54 /home/josh/.local/bin/fail -> /home/josh/.local/share/uv/tools/fail/bin/fail
```

>Just curious, does this work?
> ```sh
> uv tool install . --reinstall
> ```

Well yes, but I don't want to use `--reinstall`. `--reinstall-package` should be enough (implied by `--force`. In practice, I have expensive to install dependencies and want to make this command as lightweight as possible. But it is good to note and add, that running the full fat `--reinstall` does have the expected behvior.


---
