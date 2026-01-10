```yaml
number: 15104
title: Is uv cache clean thread-safe / process-safe?
type: issue
state: closed
author: mil-ad
labels:
  - question
assignees: []
created_at: 2025-08-06T10:33:52Z
updated_at: 2025-08-30T22:43:40Z
url: https://github.com/astral-sh/uv/issues/15104
synced_at: 2026-01-10T03:23:54Z
```

# Is uv cache clean thread-safe / process-safe?

---

_Issue opened by @mil-ad on 2025-08-06 10:33_

### Question

Is it safe to run `uv cache clean` when there's potentially another process running uv operations? Does it acquire fnlock or something similar? 

Why? I have multiple users using uv (and each setting UV_CACHE_DIR) and I want to have a cron job occasionally cleaning caches

### Platform

amd64

### Version

uv 0.8.5

---

_Label `question` added by @mil-ad on 2025-08-06 10:33_

---

_Comment by @hutch3232 on 2025-08-06 13:03_

https://docs.astral.sh/uv/concepts/cache/#cache-safety

> Note that it's not safe to modify the uv cache (e.g., uv cache clean) while other uv commands are running, and never safe to modify the cache directly (e.g., by removing a file or directory).

---

_Comment by @hutch3232 on 2025-08-06 13:27_

I'm in a similar situation to you, where I have multiple users sharing one cache. We've done this by having everyone set `UV_CACHE_DIR` to the same value. Over time, the size of the cache has blown up, so I was hoping to get rid of old unused installs. Similar to you, I saw `uv cache prune` which:

> uv cache prune removes all unused cache entries. For example, the cache directory may contain entries created in previous uv versions that are no longer necessary and can be safely removed. uv cache prune is safe to run periodically, to keep the cache directory clean.

That sounded like it would be safe in my case, since the cache entries were "unused", so I ran it. Unfortunately it broke a bunch of users venv's. They had errors like "module not found" but it would be for like submodules (specific error example I saw: No module named 'pytz.tzinfo' when importing pandas). It was pretty disruptive - oops - and the solution was to have each user run `rm -rf .venv/ && uv sync`. Simply running `uv sync` did not work - it said it was still in-sync. Possibly relevant detail: we are also setting `UV_LINK_MODE=symlink`. I am thinking that this command `uv cache prune` really only makes sense when you are using the standalone / system-level install, instead of `uv` being installed into the venv itself. The reason is that with the system-level install, you have a consistent version of `uv` (and thus the cache versions) so it is safe to remove old cache version archives... but when users are all using different versions of `uv`, removing cache entries for prior `uv` versions isn't safe for users still on old versions of `uv`. 

I have two thoughts coming out of this experience. Maybe the docs could be elaborated to explain what "unused cache entries" means specifically. Also, perhaps `uv sync` could have an optional setting to more thoroughly check... perhaps by following symlinks or something. Note: my initial attempt to fix this was with `uv sync --refresh` and that didn't work. Only removing the whole venv and resyncing worked.

---

_Comment by @powercoconola on 2025-08-06 17:25_

> We've done this by having everyone set `UV_CACHE_DIR` to the same value.

Is the cache directory a spot on the network drive or something? How is it that multiple users can access the same cache directory? Did you set it to a global spot on a drive instead of a user-directory?

We just leave the cache dir as default, as mentioned in docs:

> A system-appropriate cache directory, e.g., $XDG_CACHE_HOME/uv or $HOME/.cache/uv on Unix and %LOCALAPPDATA%\uv\cache on Windows

---

_Comment by @hutch3232 on 2025-08-06 18:11_

Good questions - I might be in a strange situation. I will say it has worked incredibly well for us except for this one-off user error situation.

Basically we are using python in containerized workspaces. These workspaces are largely ephemeral - if we left the cache as the default location they'd be wiped out on shutdown. For significant time savings, we created a drive used to store the `uv` cache which we then mount to each ephemeral workspace. These are linux (ubuntu) environments built using Docker fwiw. When running the image, we're all the same "user" i.e., `$USER`, which is how we don't get into horrible unix permissions issues situation when installing into the same cache location. 

---

_Comment by @konstin on 2025-08-06 18:53_

@hutch3232 I'm sorry you've been affected by this, we had a warning about this case specifically but it got cut out of the rendered docs: https://github.com/astral-sh/uv/issues/15115.

Regarding the original question, it's not safe to prune or clear the cache while other uv commands are running, but (with the notable exception of `UV_LINK_MODE=symlink`) it is safe to run `uv cache clear` or `uv cache prune` for cache maintenance without breaking existing venvs.

---

_Comment by @hutch3232 on 2025-08-06 19:16_

Thanks, @konstin! The behavior makes sense. I wish I could use hardlinks. When I tried a year ago it didn't work - presumably because of my mounted drive setup. Seems like a me-problem.

---

_Comment by @mil-ad on 2025-08-07 08:06_

> Is the cache directory a spot on the network drive or something? How is it that multiple users can access the same cache directory? Did you set it to a global spot on a drive instead of a user-directory?

@powercoconola  It doesn't have to be multiple users (though uv cache is additive so sharing it among multiple users shouldn't be an issue?). The same user could be running multiple jobs in parallel. 


---

_Comment by @charliermarsh on 2025-08-30 22:43_

Yeah, we consider it unsafe to clear the cache while other users are running commands (per the docs).

---

_Closed by @charliermarsh on 2025-08-30 22:43_

---
