```yaml
number: 4411
title: Store Python toolchains under XDG directory on macOS
type: issue
state: closed
author: baggiponte
labels:
  - configuration
  - macos
assignees: []
created_at: 2024-06-19T13:24:45Z
updated_at: 2024-08-20T16:31:48Z
url: https://github.com/astral-sh/uv/issues/4411
synced_at: 2026-01-10T04:53:49Z
```

# Store Python toolchains under XDG directory on macOS

---

_Issue opened by @baggiponte on 2024-06-19 13:24_

Hey there! Thanks for the amazing work. Using `uv` preview features and love it.

I noticed `uv toolchain` installs under `$HOME/Library/Application Support`. How does `uv ` feel about following XDG spec? I saw ruff is doing it. For example, `rye` installs under `$HOME/.local/share`.

Also when I do `uv toolchain list` I get this result:

```bash
warning: `uv toolchain list` is experimental and may change without warning.
cpython-3.12.4-macos-aarch64-none       /opt/homebrew/opt/python@3.12/bin/python3.12
cpython-3.12.3-macos-aarch64-none       /Users/luca/Library/Application Support/uv/toolchains/cpython-3.12.3-macos-aarch64-none/install/bin/python3
cpython-3.11.9-macos-aarch64-none       /Users/luca/Library/Application Support/uv/toolchains/cpython-3.11.9-macos-aarch64-none/install/bin/python3
cpython-3.10.14-macos-aarch64-none      /Users/luca/Library/Application Support/uv/toolchains/cpython-3.10.14-macos-aarch64-none/install/bin/python3
cpython-3.9.19-macos-aarch64-none       /Users/luca/Library/Application Support/uv/toolchains/cpython-3.9.19-macos-aarch64-none/install/bin/python3
cpython-3.9.6-macos-aarch64-none        /Library/Developer/CommandLineTools/usr/bin/python3
cpython-3.8.19-macos-aarch64-none       /Users/luca/Library/Application Support/uv/toolchains/cpython-3.8.19-macos-aarch64-none/install/bin/python3
```

i.e. also Homebrew Python.

(I guess a `uv pin` or `uv local` command is coming soon, looking forward to it.)

---

_Comment by @zanieb on 2024-06-19 14:37_

We're talking about it e.g. https://github.com/astral-sh/uv/pull/2236 and https://github.com/astral-sh/uv/pull/3049 and https://github.com/astral-sh/ruff/issues/10739

I think it's fairly clear that people prefer the XDG directory for configuration on macOS but I'm not sure what to think of application "state" like this.

---

_Label `configuration` added by @zanieb on 2024-06-19 14:37_

---

_Label `macos` added by @zanieb on 2024-06-19 14:37_

---

_Comment by @zanieb on 2024-06-19 14:49_

Personally I'm pretty interested it's just weird to go against the platform standard and notably our cache is also not using the XDG directory on macOS.

---

_Comment by @baggiponte on 2024-06-19 14:52_

Thanks for the prompt reply! As far as Python interpreter discovery, what do you plan to support? Anyway, feel free to close this 😊

---

_Comment by @zanieb on 2024-06-19 14:55_

I'm happy to leave it open to gauge interest. Maybe we need a special toggle.

Python interpreter discovery is a whole thing :) I don't think we should chat about it in the issue, but you could take a look at the [supported sources](https://github.com/astral-sh/uv/blob/57a58957440600dffcb361394850187764d373b4/crates/uv-toolchain/src/discovery.rs#L128-L145). https://github.com/astral-sh/uv/issues/4198 will address your question about the system interpreters being listed.

---

_Comment by @dsully on 2024-06-20 01:16_

+1 for `~/.local`, etc. Effectively treating macOS like Linux in this regard.

---

_Comment by @hoechenberger on 2024-06-20 05:01_

Just chiming in here to say that I'd rather keep the current behavior. XDG is not relevant for macOS and I'd like uv to follow the platform's standards, not those of a Linux system. That said, I have not yet read any of the referenced issues / discussions.

---

_Comment by @baggiponte on 2024-06-20 09:05_

> Just chiming in here to say that I'd rather keep the current behavior. XDG is not relevant for macOS and I'd like uv to follow the platform's standards, not those of a Linux system. That said, I have not yet read any of the referenced issues / discussions.

I see your point, but there are already plenty of programs that, if XDG_* vars are set on macos, will comply with that. It's handy because as a developer it improves the way I manage things (dotfiles, knowing where to go when I need to free up ram...). I don't remember how it ended up there, but there are a bunch of `jetbrains` stuff under my `~/.local/share` too.

It's also fair to say there are plenty of programs that don't care. VSCode, for example, slams everything under `$HOME`, IIRC plugins included.

EDIT: My point is: it feels it's more of a standard for GUIs/Applications than programs.

---

_Comment by @hoechenberger on 2024-06-20 10:18_

@baggiponte I absolutely agree that many programs don't respect the OS conventions 😃 When developing Python packages, for example, I therefore make use of [`platformdirs`](https://pypi.org/project/platformdirs/), which automatically does "the right thing" when it comes to figuring out where to store config files, temporary data etc. I'd argue it's good practice to follow this approach ... I'm not looking to start a fight over this :) just wanted to share my opinion.

best wishes!
Richard 

---

_Comment by @dsully on 2024-06-20 11:16_

To be clear - as long as the `$XDG_*` variables are respected as @baggiponte says, I'm good with that approach.

---

_Comment by @hoechenberger on 2024-06-20 11:31_

Ah! Got you! Yes this does make sense.

---

_Comment by @charliermarsh on 2024-06-23 15:56_

For context, we _do_ use XDG for configuration file discovery so there's some precedent there.

---

_Renamed from "Follow XDG spec on macOS (?)" to "Store Python toolchains under XDG directory on macOS (?)" by @charliermarsh on 2024-06-23 15:56_

---

_Renamed from "Store Python toolchains under XDG directory on macOS (?)" to "Store Python toolchains under XDG directory on macOS" by @charliermarsh on 2024-06-23 15:56_

---

_Comment by @zanieb on 2024-06-24 21:10_

Another datapoint is that `pipx` uses the XDG spec for storing installed tools.

---

_Comment by @baggiponte on 2024-06-29 21:18_

> Another datapoint is that `pipx` uses the XDG spec for storing installed tools.

Speaking of, tools that are installed with `uv tool install` will have to be added to `$PATH` somehow. I don't think I ever saw adding `$HOME/Library/Application Support/uv/` to `$PATH`. IIRC, `rye` prompts you to use shims.

---

_Comment by @zanieb on 2024-06-29 22:47_

Tool installation places binaries entry points according to the XDG specification on all platforms https://github.com/astral-sh/uv/blob/13b0beb56fdc607c1f38d820dbf8c95c8fd0ce84/crates/uv-tool/src/lib.rs#L264-L275

But that doesn't mean we need to place the virtual environment there.

Regardless, I'm leaning towards switching to XDG entirely on macOS.

---

_Comment by @charliermarsh on 2024-06-30 17:05_

@zanieb - We still may need something like `pipx ensurepath` for all this stuff though -- it's not guaranteed that they're on the user PATH IIUC.

---

_Comment by @zanieb on 2024-06-30 22:01_

Yeah of course — that's described in #3560 but I'll create a specific issue.

edit: We should at least do #4671 

---

_Comment by @zanieb on 2024-07-03 13:52_

I've opened a pull request to support the variable if set https://github.com/astral-sh/uv/pull/4769

---

_Assigned to @zanieb by @charliermarsh on 2024-07-30 15:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-30 15:39_

---

_Unassigned @zanieb by @charliermarsh on 2024-07-30 15:39_

---

_Closed by @zanieb on 2024-08-20 16:31_

---
