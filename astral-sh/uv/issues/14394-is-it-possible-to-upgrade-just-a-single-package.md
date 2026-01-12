```yaml
number: 14394
title: Is it possible to upgrade just a single package if you change exclude-newer?
type: issue
state: closed
author: alarthast
labels:
  - question
assignees: []
created_at: 2025-07-01T14:27:34Z
updated_at: 2025-08-20T13:08:45Z
url: https://github.com/astral-sh/uv/issues/14394
synced_at: 2026-01-12T16:01:48Z
```

# Is it possible to upgrade just a single package if you change exclude-newer?

---

_@alarthast_

### Question

The default behaviour of `uv` is to prefer the previously locked versions of packages if there is an existing `uv.lock` file[^1]. However, if we use the `--exclude-newer` option, then upgrading a single package also upgrades all other packages:

i.e.
```bash
uv lock --exclude-newer <yesterday>
uv lock --exclude-newer <today> --upgrade-package foo
```
will lead to something like:
```
Ignoring existing lockfile due to change in timestamp cutoff: `2025-06-30T23:00:00Z` vs. `2025-07-01T23:00:00Z`
```
And the attempt to upgrade a single package will upgrade all packages with the new timestamp.

Is there a way to upgrade a single package with a newer timestamp, while preserving the versions of existing locked packages? The only way we have found to do this is to manually edit the timestamp in the lockfile before updating a single package, which is obviously not recommended.

Our use-case is that we use `--exclude-newer` to implement a 7 day cool-off period for upgrading dependencies in our projects. However, on occasion there are critical security updates to a dependency that we want to upgrade without waiting for the 7 day cool-off. We’d like to be able to update occasional urgent exceptions (and their required transitive dependencies) to a more recent version without simultaneously upgrading all the other packages.


[^1]: https://docs.astral.sh/uv/concepts/projects/sync/#upgrading-locked-package-versions



### Platform

Linux 6.8.0-60-generic x86_64 GNU/Linux

### Version

uv 0.7.17

---

_Label `question` added by @alarthast on 2025-07-01 14:27_

---

_Comment by @charliermarsh on 2025-07-02 00:52_

Yeah we ignore the existing versions entirely if the `--exclude-newer` timestamp changes, though we don't necessarily have to -- we could respect them as preferences. I'm worried that behavior would lead to more user-surprise, though, since incrementing the `--exclude-newer` wouldn't cause any dependencies to get upgraded by default.


---

_Comment by @alarthast on 2025-07-02 15:48_

Thanks for answering! We're certainly interested for there to be some way of doing a single package upgrade with `--exclude-newer`, but see why that hadn't been made an option so far. (If there is some workaround that we have missed, please kindly let us know!)

---

_Comment by @zanieb on 2025-07-02 15:50_

I guess the easiest fix within the existing design would be to support `exclude-newer-package`

---

_Comment by @konstin on 2025-07-28 10:09_

> Yeah we ignore the existing versions entirely if the `--exclude-newer` timestamp changes, though we don't necessarily have to -- we could respect them as preferences. I'm worried that behavior would lead to more user-surprise, though, since incrementing the `--exclude-newer` wouldn't cause any dependencies to get upgraded by default.

I would find it more consistent to not invalidate the lockfile when exclude-newer changes; iirc the current design happened because we did not consider use cases the cutoff moves in the initial design. It is already possible to invalidate the lockfile with `--upgrade`, but there's no equivalent `--no-upgrade` to avoid invalidating the lockfile. Upgrading when changing `--exclude-newer` is opposite to how other options work, which avoid changing the lockfile as much as possible.

---

_Closed by @zanieb on 2025-07-29 22:00_

---

_Comment by @alarthast on 2025-08-20 13:07_

On behalf of our team, thank you so much for releasing the `--exclude-newer-package` feature! We have tested it and are happy with how it's working :D 

---

_Comment by @charliermarsh on 2025-08-20 13:08_

Awesome, thanks for following up!

---
