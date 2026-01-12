```yaml
number: 11958
title: "Support for `UV_GROUP` environment variable that acts like the CLI `--group` argument"
type: issue
state: open
author: iloveitaly
labels:
  - enhancement
assignees: []
created_at: 2025-03-04T18:32:45Z
updated_at: 2025-12-11T00:33:03Z
url: https://github.com/astral-sh/uv/issues/11958
synced_at: 2026-01-12T16:00:50Z
```

# Support for `UV_GROUP` environment variable that acts like the CLI `--group` argument

---

_@iloveitaly_

### Summary

Right now, the only way to add an additional group to install to the uvsync command is to manually add the group as a CLI argument:

```
uv sync --group debugging-extras
```

It would be great if this argument could be added via environment variables as well. Or, even better, maybe allow additional CLI args to be specified via a generic var like `UV_CLI_ARGS`.

This is somewhat related to:

* https://github.com/astral-sh/uv/issues/11183
* https://github.com/astral-sh/uv/issues/10934

### Example

```
UV_GROUP=debugging-extras
```

Or, even better, the ability to specify multiple groups:

```
UV_GROUP=debugging-extras,other-group
```

The most flexible solution would be something like:

```
UV_CLI_ARGS='--group debugging-extras --group other-group`
```

The benefit here is being able to install an additional group by default without changing the code committed to a Git repository. To be more concrete, imagine a developer setup where you want additional debugging tools installed, but you do not want those tools installed on CI, staging, or other environments where you would install the `dev` group.

---

_Label `enhancement` added by @iloveitaly on 2025-03-04 18:32_

---

_Comment by @zanieb on 2025-03-04 18:36_

I would recommend changing the default groups and using `--no-default-groups --dev` in CI etc

We can consider supporting `UV_GROUP` though.

---

_Comment by @iloveitaly on 2025-03-05 18:57_

The problem with that approach, is you need to modify your build system, which isn't always possible (in the case of `nixpacks` you'd have to run a custom-built rust binary in your deployment pipeline).

---

_Comment by @zanieb on 2025-03-05 19:05_

> The problem with that approach, is you need to modify your build system, which isn't always possible 

Like, you can't pass the arguments through?

---

_Comment by @iloveitaly on 2025-03-06 18:04_

Nope:

https://github.com/railwayapp/nixpacks/blob/bcd9bfe0a2005e0f5a72f96f385579eae5052e5c/src/providers/python.rs#L254-L256

---

_Comment by @jvacek on 2025-10-08 14:07_

I would really appreciate having an env var equivalents to `--no-default-groups`, `--group`, `--only-group` etc.

Currently, in our CI, I need to define variables for these, pass them through to the job, and then deal with the absence/presence at shell level.

Absorbing the env vars would be far cleaner in my opinion.

---

_Comment by @rtpg on 2025-12-11 00:33_

In our usecase, people have basically trained themselves to type `uv sync` to update stuff.

 We have some optional dependency groups that don't _really_ belong in `dev` but are useful for most local setups. And, importantly, because we use `direnv` we have a straightforward way to roll out environment variable changes to local setups.

Being able to provide the "default" targets for `uv sync` through env vars would be nice. Though one could argue "stop calling `uv sync` directly!" is a real alternative.

I suppose the guiding light here is that while CI and friends are automated, local dev tends to be people tying in things manually, so in those situations the more we can configure things smoothly to roll them out to other members of the team, the better.

or shorter: does the existence of `UV_NO_GROUP` not imply the existence of `UV_GROUP`? 😄 

---
