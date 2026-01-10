```yaml
number: 7323
title: "generate-shell-completion: generate `uvx` shell completions for the fish shell"
type: pull_request
state: closed
author: ilyagr
labels: []
assignees: []
base: main
head: fish-uvx
created_at: 2024-09-12T05:09:54Z
updated_at: 2024-09-16T19:55:26Z
url: https://github.com/astral-sh/uv/pull/7323
synced_at: 2026-01-10T12:53:45Z
```

# generate-shell-completion: generate `uvx` shell completions for the fish shell

---

_Pull request opened by @ilyagr on 2024-09-12 05:09_

Cc: #7258

## Summary

`uv generate-shell-completion fish` will automatically include completion for `uvx`.

Hopefully, others will contribute something similar to other shells.

## Test Plan

I ran

     cargo run -- generate-shell-completion fish |source

in `fish` and checked `uvx --f<TAB>` expands to the correct options. You can also do `uvx -<TAB>`.

---

_Marked ready for review by @ilyagr on 2024-09-12 05:16_

---

_Comment by @ilyagr on 2024-09-12 16:06_

Another approach to this would be to make a `clap::Command` for `uvx` and call `clap_complete::generate` on it. That would work for all shells. I haven't yet looked into whether this would be easy to do; something like this `clap::Command` might already exist to make `uvx` work in the first place.

---

_Comment by @bluss on 2024-09-12 16:49_

I think that for bash, you want the uv and uvx completion to end up in different files, because completion is lazy loaded by filename.

There might be some way to work around, that, however -- the [recommended way](https://github.com/scop/bash-completion/blob/main/README.md) seems to be to put the whole completion in a file called `uv` and symlink `uvx` -> `uv` in the completions dir.   That should work, but it's still a solution that requires two files. :neutral_face: 

I know the PR is about another shell, just bringing this up as a concern that could need to be addressed in general in the design.

---

_Comment by @bluss on 2024-09-12 16:56_

The concern about filename that I had for bash, probably applies in a similar way to fish:

> A completion file must have a filename consisting of the name of the command to complete and the suffix .fish.

https://fishshell.com/docs/current/completions.html

---

_Comment by @ilyagr on 2024-09-12 20:46_

> > A completion file must have a filename consisting of the name of the command to complete and the suffix .fish.
> 
> https://fishshell.com/docs/current/completions.html

I think this applies if you want to ship the completions together with the fish shell. I don't think it matters if you follow [the instructions](https://docs.astral.sh/uv/getting-started/installation/#shell-autocompletion) and do

```
echo 'eval "$(uv generate-shell-completion bash)"' >> ~/.bashrc
echo 'uv generate-shell-completion fish | source' >> ~/.config/fish/config.fish
```

I think my statement applies to `bash` as well, `eval "$(uv generate-shell-completion bash)"` looks like it will initialize the completion eagerly and that there is no file to call `uv.bash`. However, I'm less familiar with bash, so I might be missing something.

-----------

One situation where your concerns might be much more relevant is for people who package `uv` for various distributions (Arch, Debian, Chocolatey,...). They might include the shell completions in files as opposed to running `uv generation-shell-completion` every time, and they would probably want to follow guidelines closer to what you describe. So, perhaps more should be done to support them at some point, e.g. `uv generate-shell-completion --uv-only` and `uv generate-shell-completion --uvx-only`. Or we could wait for such people to show up and request something, I'm just guessing what they might need.

---

_Comment by @bluss on 2024-09-12 21:37_

I didn't know the docs had those instructions, I can see how it makes it work.

I prefer to use bash completions this way and would recommend it to others:

```bash
BASH_COMPLETION_DIR="$HOME/.local/share/bash-completion/completions"
mkdir -p "$BASH_COMPLETION_DIR"
uv generate-shell-completion bash > "$BASH_COMPLETION_DIR"/uv
```

It sets up lazy loading correctly. If every tool wanted to be called every time you open a shell (to generate completions), it would take a long time, it's not sustainable.

---

_Comment by @ilyagr on 2024-09-12 22:39_

> If every tool wanted to be called every time you open a shell (to generate completions), it would take a long time, it's not sustainable.

I would have thought so, but my understanding is that `fish` seems to load all the completions eagerly, and seems to work fine and open quickly. It's magic. I'm guessing that `bash` is just as fast. **Update:** Or maybe not, judging by bluss's comment below, which is a quote from https://fishshell.com/docs/current/completions.html.

If somebody has a better understanding, let me know!

---

_Comment by @bluss on 2024-09-13 17:24_

This makes it sound like lazy loading, not eager, "*and any completions defined are automatically loaded when needed.*"

Now that we know about bash's symlinks, assuming that works for fish too, maybe it's just fine to combine both completion scripts in the same output? It will work with uv's instructions. And the more detailed setups with a file per command can use a symlink from uvx.fish -> uv.fish and so on.

---

_Comment by @bluss on 2024-09-14 09:30_

There is a proof of concept solution here https://github.com/astral-sh/uv/pull/7388 but it has some small technical complications

---

_Comment by @ilyagr on 2024-09-16 01:38_

I like the approach of #7388, since it will work for all shells. I haven't thought about the questions you asked in that PR much. I'm happy to close this PR in favor of #7388 if that PR has a promising future.

---

_Comment by @ilyagr on 2024-09-16 19:55_

I'm closing this in favor of #7388.

---

_Closed by @ilyagr on 2024-09-16 19:55_

---
