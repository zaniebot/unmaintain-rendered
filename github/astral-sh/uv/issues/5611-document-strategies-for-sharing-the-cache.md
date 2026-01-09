---
number: 5611
title: Document strategies for sharing the cache
type: issue
state: open
author: zanieb
labels:
  - documentation
  - cache
assignees: []
created_at: 2024-07-30T17:16:47Z
updated_at: 2025-12-10T04:21:55Z
url: https://github.com/astral-sh/uv/issues/5611
synced_at: 2026-01-07T13:12:17-06:00
---

# Document strategies for sharing the cache

---

_Issue opened by @zanieb on 2024-07-30 17:16_

Per https://github.com/astral-sh/uv/issues/5581#issuecomment-2258832686, sharing the cache across users can be useful for system-wide uv installations.

---

_Label `documentation` added by @zanieb on 2024-07-30 17:16_

---

_Label `cache` added by @zanieb on 2024-07-30 17:16_

---

_Referenced in [astral-sh/uv#5581](../../astral-sh/uv/issues/5581.md) on 2024-07-30 17:18_

---

_Comment by @stas00 on 2024-07-31 03:53_

Here is the summary so far of what needs to be done to make `uv` work with a shared cache - this helps to save a ton of disk space and inodes when there are many users on a system and files can be shared.

1. add to  `~/.bashrc` or `~/.profile` of each user:

```
umask 000 # file creation so that all users's files are a+rw or a+rwx
export UV_LINK_MODE=copy # hardlinking doesn't work with different owners
```
2. Then create a shared cache folder:
```
sudo mkdir -p /data/cache/uv
sudo chmod a+rwx /data/cache/uv
```
3. Then each user redirecting their local cache to the shared one:
```
rm -r ~/.cache/uv
ln -s /data/cache/uv ~/.cache/uv
```

and this requires `uv>=0.2.32` to work

Done.

---

_Comment by @zanieb on 2024-07-31 04:05_

Thanks! If you're setting environment variables anyway, you can use `UV_CACHE_DIR` and skip the symbolic link.

---

_Comment by @stas00 on 2024-07-31 05:17_

oh, that's super-useful, then I don't have to go after users and making sure they installed a symlink.

Thank you very much, @zanieb!

---

_Comment by @paveldikov on 2024-07-31 10:43_

Worth mentioning (or even discussing!) the security implications of this. (cache poisoning can be a huge problem)

---

_Comment by @MattiasDC on 2024-08-09 12:40_

If you can't or don't want to use umask (as it will change behavior it for all files you create, not only uv cache). You can also use ACL rules on the cache directory, which overrides umask configuration.

---

_Comment by @rabin-io on 2024-11-11 16:57_

What if you have many users WRITING to the cache? e.g. starting from a clean folder, and invoking many CI/test jobs at the same time which might want to download their files.

---

_Comment by @chrispy-snps on 2025-01-12 23:39_

The `umask` is a significant problem with shared caches. In a corporate environment, most users have their `umask` set to grant write permissions only to SELF, but a shared cache requires that the cache files be writable by SELF+GROUP (assuming all cache users share a group).

Setting `umask` to alow SELF+GROUP write permissions account-wide is a non-starter.

My company develops UNIX/Linux software that implements a shared cache across many users. It has been used successfully for over 30 years. The key is that our software explicitly `chmod`s cache-specific permissions when it creates cache files and directories, thus overriding (and thus not being restricted by) the default permissions resulting from the account `umask`:

```
shared_cache_dir_path = "/path/to/cache"
shared_cache_dir_chmod_octal = "770"
shared_cache_file_chmod_octal = "660"
```

I think that `uv` would need to use a solution like this. And it might be challenging if non-`uv` subprocesses also create files and directories, as all of those would need to be explictly `chmod`ed too.

---

_Comment by @grst on 2025-01-13 06:51_

> ```
> export UV_LINK_MODE=copy # hardlinking doesn't work with different owners
> ```

Doesn't this defy the purpose of having a shared cache? Maybe the chache size will be reduced, but the individual users' `.venv` will consume much more space. 



---

_Comment by @chrispy-snps on 2025-01-13 10:48_

@grst - that was exactly my thought too.

---

_Comment by @observerw on 2025-02-06 10:16_

> Here is the summary so far of what needs to be done to make `uv` work with a shared cache - this helps to save a ton of disk space and inodes when there are many users on a system and files can be shared.
> 
> 1. add to  `~/.bashrc` or `~/.profile` of each user:
> 
> ```
> umask 000 # file creation so that all users's files are a+rw or a+rwx
> export UV_LINK_MODE=copy # hardlinking doesn't work with different owners
> ```
> 
> 2. Then create a shared cache folder:
> 
> ```
> sudo mkdir -p /data/cache/uv
> sudo chmod a+rwx /data/cache/uv
> ```
> 
> 3. Then each user redirecting their local cache to the shared one:
> 
> ```
> rm -r ~/.cache/uv
> ln -s /data/cache/uv ~/.cache/uv
> ```
> 
> and this requires `uv>=0.2.32` to work
> 
> Done.

Setting `umask` to 000 does not seem very secure. Inspired by [this article](https://zhuanlan.zhihu.com/p/570747928), a better approach is to manage shared cache permissions by creating a user group.

First, create a new user group named `share`:

```bash
groupadd share
```

Then, create a shared cache directory on the storage device, transfer its group ownership to the `share` group, and allow group members to read and write:

```bash
mkdir /data/.uv_cache # Or move an existing .uv_cache: mv <SOURCE>/.uv_cache /data/
chgrp -R share /data/.uv_cache
chmod -R g=rwX,o-rwx /data/.uv_cache # Allow group members to read/write but deny access to others
```

Set the SGID bit on all subdirectories to ensure that all newly created directories inherit the group ownership:

```bash
find /data/.uv_cache -type d -exec chmod g+s {} +
```

Next, modify `/etc/uv/uv.toml` to point the cache directory to this location. Note that if `/data` is on a different storage device than the user directory, `uv` 's default hard link behavior will not work, so `link-mode` needs to be set to symbolic links:

```toml
# /etc/uv/uv.toml

cache-dir="/data/.uv_cache"
link-mode="symlink" # when hardlink is not possible
```

(I noticed that all `uv` environment variables have their counterparts in [Settings](https://docs.astral.sh/uv/reference/settings/), so there is no need to export the environment variables separately, just change the settings)

Finally, add users who need access to the shared cache to the `share` group:

```bash
adduser share <USER_NAME>
```

If you want new users to be added to the `share` group by default, add the following lines to `/etc/adduser.conf`:

```conf
EXTRA_GROUPS="share"
ADD_EXTRA_GROUPS=1
```

If you find any problems pls let me know 😄

---

_Comment by @chrispy-snps on 2025-02-06 11:38_

@observerw - commands like `groupadd` and `adduser` require root privileges. This can be a blocking issue in corporate environments.

---

_Referenced in [astral-sh/uv#13323](../../astral-sh/uv/issues/13323.md) on 2025-05-07 22:13_

---

_Comment by @woutervh on 2025-05-07 22:43_

> Doesn't this defy the purpose of having a shared cache? 
> Maybe the chache size will be reduced, 
> but the individual users' .venv will consume much more space.

A possible issue not yet mentioned:

if someone needs to troubleshoot a python application running in a venv  and sets a pdb-breakpoint in a library, 
you do not want to block all other python-applications running in other venvs.

The first priority of a venv is to provide isolation from other venvs.




---

_Referenced in [astral-sh/uv#12875](../../astral-sh/uv/issues/12875.md) on 2025-06-03 03:08_

---

_Comment by @oraluben on 2025-06-03 03:32_

@observerw Amazing instructions, this is exactly what I'm looking for.

One small nit:

> Finally, add users who need access to the shared cache to the `share` group:
> 
> adduser share <USER_NAME>


Adding user to group should be: `usermod -a -G share <user>`

---

_Referenced in [ecmwf/WeatherGenerator#344](../../ecmwf/WeatherGenerator/issues/344.md) on 2025-06-25 10:16_

---

_Comment by @yaxiongzhao-genesis on 2025-12-10 04:21_

(I am sorry)
But where is **the instruction**? I saw links to a few places, but I'd expect a link to an official page on uv's home page?
Or this doc is still in prep?

---
