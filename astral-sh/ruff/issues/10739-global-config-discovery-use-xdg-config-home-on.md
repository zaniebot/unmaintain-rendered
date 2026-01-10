```yaml
number: 10739
title: "Global Config Discovery: use `$XDG_CONFIG_HOME` on macOS"
type: issue
state: closed
author: SPiCaRiA
labels:
  - configuration
assignees: []
created_at: 2024-04-02T14:09:39Z
updated_at: 2024-06-27T11:44:13Z
url: https://github.com/astral-sh/ruff/issues/10739
synced_at: 2026-01-10T11:09:53Z
```

# Global Config Discovery: use `$XDG_CONFIG_HOME` on macOS

---

_Issue opened by @SPiCaRiA on 2024-04-02 14:09_

Hi, I'm newly migrating to ruff and it works so far so good for me, thank you for making this great tool!

However, I noticed that ruff uses the `config_dir` in `dirs` crates, which goes to `$HOME/Library/Application Support` under macOS. Generally I believe that as a CLI tool (and like many others), ruff should probably stick with XDG config home under macOS. When I look into my `$HOME/Library/Application Support`, they're almost all GUI application stuffs. Also, I think it is expected & intuitional to read config from `$XDG_CONFIG_HOME` since macOS mostly behave in *nix way.

Please let me know what you think :)

---

_Label `configuration` added by @zanieb on 2024-04-02 14:11_

---

_Comment by @zanieb on 2024-04-02 14:11_

This seems reasonable to me but I'd like to hear from someone else on our team first too.

---

_Comment by @BurntSushi on 2024-04-02 14:39_

We have a similar request in a comment for `uv` as well: https://github.com/astral-sh/uv/issues/1511

I'm not a macOS user so I don't have a good sense of what the right choice is. But my understanding is that `XDG_CONFIG_HOME` is commonly used on macOS. I have a fair number of things in my `~/.config` directory on my test mac mini for example.

---

_Comment by @zanieb on 2024-04-02 14:44_

I prefer Linux to macOS but use macOS as a daily driver and I would be very confused if `~/.config` was not a valid configuration path.

---

_Comment by @charliermarsh on 2024-04-02 16:37_

I use macOS exclusively. Honestly, I wouldn't know where to expect the config to go. I'd have to look it up in the docs for any given tool. But looking at my machine, I do think `~/.config` is more appropriate.

---

_Comment by @baggiponte on 2024-04-04 21:11_

That was a matter of discussion for PDM too (which does not recognise the XDG_* vars for macOS). PDM does this because the underlying Python package platformdirs does this. As a mac user who versions their dotfiles, I spent a bit of time to make sure that as many projects as I use can save data under `~/.config`. It makes the most sense, it's more portable.

---

_Comment by @konstin on 2024-04-17 09:29_

Could you report this upstream at https://github.com/dirs-dev/dirs-rs? I'm not a mac user so i'm not qualified to weigh in on this, i just wouldn't want to diverge from the effective rust standard without an upstream discussion first

---

_Comment by @BurntSushi on 2024-04-17 11:59_

@konstin I think it's been discussed before upstream and they are pretty opposed: https://github.com/dirs-dev/directories-rs/issues/47

I don't think I'd consider `dirs-rs` to be an effective Rust standard personally. But it is probably the most popular implementation of XDG.

There's also some other discussion that touches on this topic here: https://internals.rust-lang.org/t/pre-rfc-split-cargo-home/19747

---

_Comment by @BurntSushi on 2024-04-17 12:04_

And in particular, it looks like `dirs` won't respect `XDG_CONFIG_HOME` on macOS? https://github.com/dirs-dev/directories-rs/issues/47#issuecomment-596467168

---

_Comment by @konstin on 2024-04-17 12:32_

If we want xdg over the platform default on mac, we should consider switching to something like  [etcetera's Xdg](https://docs.rs/etcetera/latest/etcetera/app_strategy/struct.Xdg.html) and `etcetera::app_strategy::choose_app_strategy` ("Returns the current OS’s default `AppStrategy` This uses the `Windows` strategy on Windows, and `Xdg` everywhere else.") over `directories`.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-23 20:08_

---

_Closed by @MichaReiser on 2024-06-27 11:44_

---
