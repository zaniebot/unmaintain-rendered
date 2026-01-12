```yaml
number: 4424
title: "Expose `toolchain-preference` as a CLI and configuration file option"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-setting
created_at: 2024-06-20T14:49:30Z
updated_at: 2024-06-20T18:42:10Z
url: https://github.com/astral-sh/uv/pull/4424
synced_at: 2026-01-12T16:06:13Z
```

# Expose `toolchain-preference` as a CLI and configuration file option

---

_@zanieb_

Exposes the option added in #4416. Adds `--toolchain-preference` and `tool.uv.toolchain-preference` to configure if system or managed toolchains are preferred. Users can opt-out of managed toolchains or system toolchains entirely as well.

---

_Label `preview` added by @zanieb on 2024-06-20 14:51_

---

_Marked ready for review by @zanieb on 2024-06-20 16:23_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-20 16:27_

---

_Review requested from @konstin by @zanieb on 2024-06-20 16:27_

---

_Comment by @zanieb on 2024-06-20 16:27_

Should we bikeshed the name? Should it just be `--toolchains`?

---

_@BurntSushi approved on 2024-06-20 17:21_

LGTM.

As for the name... Is this something that you think will be commonly used on the CLI? If so, I think I'd favor a shorter name. Otherwise, I like the descriptiveness of `--toolchain-preference`.

One other possible downside is redundancy here. Namely, `--toolchain-preference prefer-system` has "prefer" twice in it, but arguably is just as clear if "prefer" only appeared once. Not quite sure what the right answer is there.

---

_Comment by @zanieb on 2024-06-20 17:23_

> One other possible downside is redundancy here. Namely, --toolchain-preference prefer-system has "prefer" twice in it, but arguably is just as clear if "prefer" only appeared once.

Agree, this is a source of discomfort for me but idk it's also very clear.

I don't expect this to be used much from the CLI — mostly as a persistent configuration option.

---

_Comment by @zanieb on 2024-06-20 17:27_

I'm fine adjusting this later if we need to since it's in preview.

---

_Comment by @zanieb on 2024-06-20 18:26_

I guess another option is I drop the `prefer` prefix so we'd have

`toolchain-preference = managed | system | only-managed | only-system | installed-managed` 

which is pretty reasonable too?

---

_Comment by @BurntSushi on 2024-06-20 18:27_

> I guess another option is I drop the `prefer` prefix so we'd have
> 
> `toolchain-preference = managed | system | only-managed | only-system | installed-managed`
> 
> which is pretty reasonable too?

I think that's probably okay. I thought of that too, but wondered whether it might leave folks wondering the difference between `managed` and `only-managed`.

---

_Merged by @zanieb on 2024-06-20 18:42_

---

_Closed by @zanieb on 2024-06-20 18:42_

---

_Branch deleted on 2024-06-20 18:42_

---
