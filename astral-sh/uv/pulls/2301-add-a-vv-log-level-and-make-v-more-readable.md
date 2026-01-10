```yaml
number: 2301
title: "Add a `-vv` log level and make `-v` more readable"
type: pull_request
state: merged
author: konstin
labels:
  - tracing
assignees: []
merged: true
base: main
head: konsti/logging
created_at: 2024-03-08T15:33:20Z
updated_at: 2024-03-11T07:58:32Z
url: https://github.com/astral-sh/uv/pull/2301
synced_at: 2026-01-10T14:49:08Z
```

# Add a `-vv` log level and make `-v` more readable

---

_Pull request opened by @konstin on 2024-03-08 15:33_

Behind error messages, the debug log is the second most important resource to finding out what and why went wrong when there was a problem with uv. It is important to see which paths it has found and how the decisions in the resolver were made. I'm trying to improve the experience interacting with the debug log.

The hierarchical layer is verbose and hard to follow, so it's moved to the `-vv` extra verbose setting, while `-v` works like `RUST_LOG=uv=debug`.

For installing jupyter with a warm cache:

* Default: https://gist.github.com/konstin/4de6e466127311c5a5fc2f99c56a8e11
* `-v`: https://gist.github.com/konstin/e7bafe0ec7d07e47ba98a3865ae2ef3e
* `-vv`: https://gist.github.com/konstin/3ee1aaff37f91cceb6275dd5525f180e
Ideally, we would have `-v`, `-vv` and `-vvv`, but we're lacking the the `info!` layer for `-v`, so there's only two layers for now.

The `tracing_subcriber` formatter always print the current span, so i replaced it with a custom formatter.

![image](https://github.com/astral-sh/uv/assets/6826232/75f5cfd1-da7b-432e-b090-2f3916930dd1)

Best read commit-by-commit.


---

_Comment by @zanieb on 2024-03-08 15:38_

Without reading through everything here.. my initial thoughts about logging are:

- `-v` should include some additional tracing messages. It should not be for debugging. It should be for users who want to understand _what_ `uv` is doing. We definitely need to show less messages than our current verbose output. A flat format makes a lot of sense.
- `-vv` should include debug level messages. It should be used for debugging. I think using a flat format is fine here too. This should be our recommended level when reporting bugs.
- `-vvv` should include trace level messages. It should be used for advanced debugging. Using the nested format makes sense here.

---

_Comment by @konstin on 2024-03-08 15:51_

My thought was to not have too many flags but have users quickly switch to `RUST_LOG=debug` or `RUST_LOG=<module>=trace`, the filtering feature is something a counted cli flag can't compete with.

---

_Comment by @zanieb on 2024-03-08 16:01_

I don't think `RUST_LOG` levels are a reasonable thing to expect from users. It seems like a power-user feature. I agree filtering seems nice but it's not like a counted flag will prevent power users from using it? We don't have to override `RUST_LOG` if it is set.

---

_Comment by @konstin on 2024-03-08 16:31_

>  We definitely need to show less messages than our current verbose output

Where would you source those messages, from what we currently show as reporter spinner status message?


---

_Closed by @konstin on 2024-03-08 16:31_

---

_Reopened by @konstin on 2024-03-08 16:31_

---

_Comment by @zanieb on 2024-03-08 16:59_

> > We definitely need to show less messages than our current verbose output
> 
> Where would you source those messages, from what we currently show as reporter spinner status message?

I'm not sure! I'm not super familiar with our logging setup. I presume we'd use `info!` level logs for things that should be shown with `-v`

---

_Comment by @konstin on 2024-03-10 15:33_

I agree, but we're currently lacking an `info!` layer, we effectively don't use it at all but only `debug!` and `trace!`. I don't want to extend the scope here even more to recategorizing all our `debug!` into `info!` and `debug!`. Instead, i'd like to go ahead with the `-v`/`-vv` split now, we can split them into three levels later.

---

_Marked ready for review by @konstin on 2024-03-10 15:33_

---

_Review requested from @zanieb by @konstin on 2024-03-10 15:33_

---

_@zanieb approved on 2024-03-11 04:11_

---

_Renamed from "Improve debug logging" to "Add a `-vv` log level and make `-v` more readable" by @konstin on 2024-03-11 07:58_

---

_Label `tracing` added by @konstin on 2024-03-11 07:58_

---

_Merged by @konstin on 2024-03-11 07:58_

---

_Closed by @konstin on 2024-03-11 07:58_

---

_Branch deleted on 2024-03-11 07:58_

---
