```yaml
number: 7388
title: "Generate shell completion for `uvx`"
type: pull_request
state: merged
author: bluss
labels:
  - cli
assignees: []
merged: true
base: main
head: uvx-complete
created_at: 2024-09-14T09:30:14Z
updated_at: 2024-09-17T19:25:55Z
url: https://github.com/astral-sh/uv/pull/7388
synced_at: 2026-01-10T12:53:46Z
```

# Generate shell completion for `uvx`

---

_Pull request opened by @bluss on 2024-09-14 09:30_

## Summary

Generate shell completion for uvx.

Create a `uvx` toplevel command just for completion by combining `uv tool uvx` (hidden alias for `uv tool run`) with global arguments. This explicit combination is needed otherwise global arguments are missing (if they are missing, clap debug assertions fail when `uv tool run` arguments refer to global arguments in directives like conflicts with).

The command `uv generate-shell-completion` now generates completion for both `uv` and `uvx` in the same output.

Fixes #7258 

## Test Plan

- Tested using bash using `eval "$(cargo run --bin uv generate-shell-completion bash)"`


---

_Comment by @bluss on 2024-09-14 09:34_

Some issues that need to be resolved

- ~~I removed global deprecated `--isolated`, otherwise it conflicts with `uvx --isolated` and this trips a runtime error. This part of the PR is (as is evident) just a placeholder, needs to be resolved~~
  - Not removed anymore. Just skipping the conflicting thing when we do shell completion.
- ~~Currently the method is uvx subcommand + TopLevelArgs. I think the better layer order would be TopLevelArgs + uvx subcommand. But I don't know clap enough to know if this makes a difference.~~
  - Resolved? because uvx + TopLevelArgs layering reproduces current `uv tool run` argument sort order
- Do all shells support combining uv and uvx completions in the same stream or file? Bash and fish (see #7323) support this.

---

_Comment by @bluss on 2024-09-14 09:36_

Normal way to install completion for a command in bash:

```bash
BASH_COMPLETION_DIR="$HOME/.local/share/bash-completion/completions"
mkdir -p "$BASH_COMPLETION_DIR"
uv generate-shell-completion bash > "$BASH_COMPLETION_DIR"/uv
```

bash lazy loads completion for the command `uv` from a file called `uv`. To support lazy loading for uvx, another file for `uvx` is needed. The recommended solution is to use a symlink from `uvx` -> `uv`

```
ln -sf "$BASH_COMPLETION_DIR"/uv "$BASH_COMPLETION_DIR"/uvx
```


The documented way to install shell completion by uv is to use this, which will continue to work unchanged:

```
eval "$(uv generate-shell-completion bash)"
```

---

_Review comment by @bluss on `crates/uv/src/lib.rs`:71 on 2024-09-14 09:38_

This point needs a better solution for the deprecated `--isolated` - need your input.

---

_@bluss reviewed on 2024-09-14 09:38_

---

_@bluss reviewed on 2024-09-14 09:38_

---

_Review comment by @bluss on `crates/uv-cli/src/lib.rs`:85 on 2024-09-14 09:38_

This was admittedly quickly thrown together and the attributes are probably overdone here.

---

_@bluss reviewed on 2024-09-14 10:14_

---

_Review comment by @bluss on `crates/uv/src/lib.rs`:803 on 2024-09-14 10:14_

We can use layer order TopLevelArgs + uvx instead like this:

```rust
            // Create uvx as a combination of top level arguments and the uv tool uvx subcommand
            let mut uvx = TopLevelArgs::command().name("uvx").bin_name("uvx").version("dummy_for_completion");
            uvx = ToolRunArgs::augment_args(uvx);
```

But that's not in the change right now, because the layer order uvx + TopLevelArgs is what produces the same argument sort order as `uv tool run`

---

_@ilyagr reviewed on 2024-09-16 01:35_

---

_Review comment by @ilyagr on `crates/uv/src/lib.rs`:803 on 2024-09-16 01:35_

I haven't tested this, but I was thinking of the following, which might be slightly easier:

```rust
#[derive(Parser)]
#[command(name = "uvx", author, long_version = crate::version::version())]
// Not all of these might be neccessary
#[command(about = "An alias for `uv tool run`.")]
#[command(
    after_help = "Use `uv help tool run` for a few more details.",
)]
pub struct UvxCli {
    #[command(flatten)]
    pub args: ToolRunArgs,

    #[command(flatten)]
    pub top_level: TopLevelArgs,
}
```

Then, you can use `args.shell.generate(&mut UvxCommand::command(), ...)`.

---

_Review comment by @bluss on `crates/uv/src/lib.rs`:803 on 2024-09-16 18:17_

This looks good, but I went with the other way because I can't resolve the `--isolated` argument name clash in this version.

---

_@bluss reviewed on 2024-09-16 18:17_

---

_@bluss reviewed on 2024-09-16 18:19_

---

_Review comment by @bluss on `crates/uv/src/lib.rs`:793 on 2024-09-16 18:19_

These three lines together look strange, but without, there are duplicate help and version. And we should really get the help and version from TopLevelArgs, because they are explicitly defined there.

---

_Renamed from "Generate shell completion for `uvx` (WIP)" to "Generate shell completion for `uvx`" by @bluss on 2024-09-16 18:20_

---

_Comment by @bluss on 2024-09-16 18:20_

Not WIP anymore because it's in shipshape - if reviewers agree, of course.

I had previously removed global `--isolated` because it tripped a debug assertion when conflicting with a different argument with the same name. But that change is now reverted in the PR, I just skip that arg when creating the synthetic `Uvx` `Command` for shell completion.

---

_@ilyagr reviewed on 2024-09-16 19:54_

---

_Review comment by @ilyagr on `crates/uv/src/lib.rs`:793 on 2024-09-16 19:54_

Minor, optional nit: I would put the key parts of this information (e.g. a quick explanation of why the duplication happens without these lines) into the code as a comment.

---

_@bluss reviewed on 2024-09-16 22:06_

---

_Review comment by @bluss on `crates/uv/src/lib.rs`:793 on 2024-09-16 22:06_

I think it's better now with a comment..

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-16 23:26_

---

_@charliermarsh approved on 2024-09-17 03:15_

This looks great, thank you.

---

_Label `cli` added by @charliermarsh on 2024-09-17 03:24_

---

_Merged by @charliermarsh on 2024-09-17 03:27_

---

_Closed by @charliermarsh on 2024-09-17 03:27_

---

_Comment by @ilyagr on 2024-09-17 03:59_

Thank you!

---

_@ilyagr reviewed on 2024-09-17 04:07_

---

_Review comment by @ilyagr on `docs/getting-started/installation.md`:184 on 2024-09-17 04:07_

I'm not the author, but my understanding is that `uv generate-shell-completion bash` currently generates the completions for both `uv` and `uvx`. I'm not sure that `uvx generate-shell-completion bash` will work at all.

If my understanding is correct, these docs are misleading, and there isn't really need for docs. The existing instructions will cause completion to work for both `uv` and `uvx`.

It wouldn't be hard to modify this code to generate the completions separately, e.g. something like `uv generate-shell-completion --uvx-only bash`. **Update:** (But I don't think there's a real need for this until somebody asks for it)

---

_@charliermarsh reviewed on 2024-09-17 04:11_

---

_Review comment by @charliermarsh on `docs/getting-started/installation.md`:184 on 2024-09-17 04:11_

You’re right, I misread the instructions in the summary. Will revert in the morning (or anyone is welcome to PR).

---

_@bluss reviewed on 2024-09-17 06:41_

---

_Review comment by @bluss on `docs/getting-started/installation.md`:184 on 2024-09-17 06:41_

We could expand the docs with the statement that uv and uvx completions are included in the same output.

Not sure if it's worth providing further hints, like if you use a per command file for completions, duplicate the output in both files or symlink uvx -> uv. Could be discussed in future docs changes.

---

_Branch deleted on 2024-09-17 08:33_

---
