```yaml
number: 7641
title: "uvx/uv tool run: Add context message before listing available tools when no arguments are provided"
type: pull_request
state: merged
author: kakkoyun
labels:
  - error messages
assignees: []
merged: true
base: main
head: add_info_message_for_empty_uvx_run
created_at: 2024-09-23T14:24:55Z
updated_at: 2024-09-27T09:45:04Z
url: https://github.com/astral-sh/uv/pull/7641
synced_at: 2026-01-10T12:53:52Z
```

# uvx/uv tool run: Add context message before listing available tools when no arguments are provided

---

_Pull request opened by @kakkoyun on 2024-09-23 14:24_

## Summary

Adds a helpful context message when `uvx` is run without arguments
To clarify, it is displaying the installed tools.

This addresses confusion, such as the one highlighted in issue #7348,
by making the output more user-friendly and informative.

Related #4024 

## Test Plan

Updated the test snapshots to include the new output.
Running the tests locally with `cargo nextest run` confirms that the tests pass.
The CI pipeline should also pass.

### Manuel Testing

**uvx**
```shell
# Make sure you have the updated version of uv installed on your path.
# cargo install --path ./crates/uv --force
❯ uvx
Provide a command to invoke with `uvx <command>` or `uvx --from <package> <command>`.

The following tools are already installed:

black v24.8.0
- black
- blackd
ruff v0.6.7
- ruff

See `uvx --help` for more information.
```

**uv tool list**
```shell
# Make sure you have the updated version of uv installed on your path.
# cargo install --path ./crates/uv --force
❯ uv tool list
black v24.8.0
- black
- blackd
ruff v0.6.7
- ruff
```

**uv tool run**
```shell
# Make sure you have the updated version of uv installed on your path.
# cargo install --path ./crates/uv --force
❯ uv tool run
Provide a command to invoke with `uv tool run <command>` or `uv tool run --from <package> <command>`.

The following tools are already installed:

black v24.8.0
- black
- blackd
ruff v0.6.7
- ruff

See `uv tool run --help` for more information.
```
---

Signed-off-by: Kemal Akkoyun <kakkoyun@gmail.com>


---

_Review requested from @zanieb by @kakkoyun on 2024-09-23 14:25_

---

_Comment by @zanieb on 2024-09-23 16:22_

I think you can be a bit more verbose here, like:

```
Provide a tool to invoke with `uvx <name>`. The following tools are already installed:

ruff v0.1.0:
- ruff
black v0.5.0:
- black
- blackd
```

There's a bit of awkwardness in the formatting of the list following the `:`.  I usually expect something like:

```
This is a list of things:
- a
- b
- c
```

This deviates a bit from the initial scope of this change, but trying to construct a prompt that puts the list in context reveals a problem with the list itself. `uvx` takes the package as a target not the executable so some of the things we're listing here aren't actually valid arguments. For example, for `blackd`:

```

❯ uv tool install black
Resolved 6 packages in 259ms
Installed 6 packages in 7ms
 + black==24.8.0
 + click==8.1.7
 + mypy-extensions==1.0.0
 + packaging==24.1
 + pathspec==0.12.1
 + platformdirs==4.3.6
Installed 2 executables: black, blackd

❯ uvx blackd
  × No solution found when resolving tool dependencies:
  ╰─▶ Because blackd was not found in the package registry and you require blackd, we can conclude that your requirements are unsatisfiable.                                           
```

You actually need `uvx --from black blackd` to use `blackd`. I'm left wondering how we can convey this to the user in this listing?

For example, should we omit the executables:

```
Provide a tool to invoke with `uvx <name>`. The following tools are already installed:
- ruff 
- black
```

Should we just suggest using `uv tool list` to see already installed tools? etc.


---

_Comment by @kakkoyun on 2024-09-23 17:21_

@zanieb Thanks for the feedback. I will have another iteration.

---

_Comment by @kakkoyun on 2024-09-23 17:26_

I also like your suggestion on omitting the details on the bare run and referring to the `uv tool list` for additional information.

Let me see what I can do.

---

_Comment by @zanieb on 2024-09-23 17:53_

Another solution would be to change the behavior itself, e.g., allow `uvx blackd` to invoke the installed `blackd` from `black` without opt-in — then the current output makes sense. I don't really want to burden you with that user-experience concern since it requires a lot of context on how `uv run` and `uv tool run` work today, but I figure it's worth pointing out while we're here. I think we can improve the current message regardless and revisit possible changes to behavior later / separately. cc @charliermarsh

---

_Comment by @kakkoyun on 2024-09-23 19:58_

@zanieb I made the context message more descriptive. As a follow-up to this PR, I can create another to improve the error message when the user searches for an executable and fails.

Let me know what you think.

---

_Review comment by @zanieb on `crates/uv/tests/tool_list.rs`:76 on 2024-09-23 21:13_

Did you mean to change `uv tool list` behavior? I don't think we should.

---

_@zanieb reviewed on 2024-09-23 21:13_

---

_Comment by @zanieb on 2024-09-23 21:26_

So, taking a deeper look at this... we're getting closer to just showing something like what Clap would show. For example, if we just make the command required Clap will show the help menu:

```diff
diff --git a/crates/uv-cli/src/lib.rs b/crates/uv-cli/src/lib.rs
index 84614fab3..913df97b8 100644
--- a/crates/uv-cli/src/lib.rs
+++ b/crates/uv-cli/src/lib.rs
@@ -3205,7 +3205,7 @@ pub struct ToolRunArgs {
     ///
     /// WARNING: The documentation for [`Self::command`] is not included in help output
     #[command(subcommand)]
-    pub command: Option<ExternalCommand>,
+    pub command: ExternalCommand,
 
     /// Use the given package to provide the command.
     ///
diff --git a/crates/uv/src/commands/tool/run.rs b/crates/uv/src/commands/tool/run.rs
index 1a120f633..3ba74c656 100644
--- a/crates/uv/src/commands/tool/run.rs
+++ b/crates/uv/src/commands/tool/run.rs
@@ -63,7 +63,7 @@ impl Display for ToolRunCommand {
 
 /// Run a command.
 pub(crate) async fn run(
-    command: Option<ExternalCommand>,
+    command: ExternalCommand,
     from: Option<String>,
     with: &[RequirementsSource],
     show_resolution: bool,
@@ -80,9 +80,9 @@ pub(crate) async fn run(
     printer: Printer,
 ) -> anyhow::Result<ExitStatus> {
     // treat empty command as `uv tool list`
-    let Some(command) = command else {
-        return tool_list(false, false, &cache, printer).await;
-    };
+    // let Some(command) = command else {
+    //     return tool_list(false, false, &cache, printer).await;
+    // };
 
     let (target, args) = command.split();
     let Some(target) = target else {
diff --git a/crates/uv/src/settings.rs b/crates/uv/src/settings.rs
index 641e780a7..60fcd5600 100644
--- a/crates/uv/src/settings.rs
+++ b/crates/uv/src/settings.rs
@@ -287,7 +287,7 @@ impl RunSettings {
 #[allow(clippy::struct_excessive_bools)]
 #[derive(Debug, Clone)]
 pub(crate) struct ToolRunSettings {
-    pub(crate) command: Option<ExternalCommand>,
+    pub(crate) command: ExternalCommand,
     pub(crate) from: Option<String>,
     pub(crate) with: Vec<String>,
     pub(crate) with_editable: Vec<String>,
```

```
❯ cargo run -q -- tool run

Run a command provided by a Python package

Usage: uv tool run [OPTIONS] <COMMAND>

Options:
   ...
```

I think we actually do lose something by not listing the installed tools there. So I do think there's something to be said about having a message here, but we need to balance the content — e.g., we can suggest the help menu for more details.

I think I'd do something like:

```
Provide a command to invoke with `uvx <command>` or `uvx --from <package> <command>`.

The following tools are already installed:

black v24.8.0
- black
- blackd

See `uvx --help` for more information.
```

I'd consider indenting or changing the formatting of the list output but that will require some sort of flag for the `tool_list` function and I'd rather avoid that here and just keep it simple.

As another note, you'll need to customize the output based on the `ToolRunCommand` variant (i.e. `uvx` or `uv tool run`).

---

_Label `error messages` added by @zanieb on 2024-09-23 21:46_

---

_Comment by @kakkoyun on 2024-09-24 11:41_

@zanieb I agree that we should use more than just the same functionality provided by Clap. The users could have already used the `help` command or the `--help` flag to explain the functionality. Suggesting the existing help commands makes perfect sense. I will make sure to keep it concise.

---

_@zanieb reviewed on 2024-09-24 12:49_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:46 on 2024-09-24 12:49_

I'd move this to just the `tool/run.rs` implementation

---

_@zanieb reviewed on 2024-09-24 12:50_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:110 on 2024-09-24 12:50_

I'd also move this to the `tool/run.rs` implementation and show "See `uv tool run --help`" when `uv tool run` is used instead of `uvx`.

---

_Review requested from @zanieb by @kakkoyun on 2024-09-24 14:26_

---

_@zanieb reviewed on 2024-09-24 16:39_

---

_Review comment by @zanieb on `crates/uv/tests/tool_list.rs`:124 on 2024-09-24 16:39_

I think these could be displayed on the same line. Should we be changing this in this pull request though? It seems like a separate change.

Similarly, we'll need to discuss if we should say "No tools installed" when there are malformed tools installed.

---

_@zanieb reviewed on 2024-09-24 16:40_

---

_Review comment by @zanieb on `crates/uv/tests/tool_list.rs`:174 on 2024-09-24 16:40_

Are we adding a trailing newline to the output?

---

_@zanieb reviewed on 2024-09-24 16:40_

---

_Review comment by @zanieb on `crates/uv/tests/tool_list.rs`:170 on 2024-09-24 16:40_

I don't think we should say this in `uv tool list`, it's implied by the purpose of the command.

---

_Review comment by @zanieb on `crates/uv/tests/tool_list.rs`:124 on 2024-09-24 16:42_

You could probably move all the changes to `uv tool list` behavior out of this and into a separate pull request.

---

_@zanieb reviewed on 2024-09-24 16:42_

---

_@kakkoyun reviewed on 2024-09-24 17:25_

---

_Review comment by @kakkoyun on `crates/uv/tests/tool_list.rs`:174 on 2024-09-24 17:25_

This is probably slipped through the cracks. I will remove it.

---

_@kakkoyun reviewed on 2024-09-24 17:28_

---

_Review comment by @kakkoyun on `crates/uv/tests/tool_list.rs`:124 on 2024-09-24 17:28_

> Similarly, we'll need to discuss if we should say "No tools installed" when there are malformed tools installed.

Shall we display a different message? Basically, I want to avoid the case where all the tools are malformed and say here are the installed tools, but they are empty. 

---

_Review comment by @zanieb on `crates/uv/tests/tool_list.rs`:124 on 2024-09-24 17:39_

Ah I see. Now I understand why you're tweaking `uv tool list`. We probably don't want to see information about malformed tools in the `uv tool run` help at all. This sort of suggests we need to stop calling the `uv tool list` implementation directly? I think generally that will make things easier, i.e. if there are no installed tools the `uv tool run` implementation can handle that directly instead.

---

_@zanieb reviewed on 2024-09-24 17:39_

---

_@zanieb reviewed on 2024-09-24 18:02_

---

_Review comment by @zanieb on `crates/uv/tests/tool_list.rs`:124 on 2024-09-24 18:02_

(this probably requires some refactor to `uv tool list` so we can avoid duplicating most of what it does?)

---

_@kakkoyun reviewed on 2024-09-24 18:33_

---

_Review comment by @kakkoyun on `crates/uv/tests/tool_list.rs`:124 on 2024-09-24 18:33_

Yes, this makes sense now. Let me try to refactor the code a little.

---

_Comment by @kakkoyun on 2024-09-24 19:58_

@zanieb I hope that with the 903c77ad92438cc0b5fcbb31f5ee76ec0f07dbfb, I finally captured what you have in mind 😅 For some reason, I was under the impression that we want to keep the behavior in sync with `uv tool list.` Now that I think about it, it's because the code reuse was already in place.

---

_Renamed from "Add context message before listing available tools when no arguments are provided" to "uvx/uv tool run: Add context message before listing available tools when no arguments are provided" by @kakkoyun on 2024-09-25 17:45_

---

_Review requested from @zanieb by @kakkoyun on 2024-09-25 19:55_

---

_@zanieb approved on 2024-09-26 20:34_

---

_Merged by @zanieb on 2024-09-26 20:35_

---

_Closed by @zanieb on 2024-09-26 20:35_

---

_Branch deleted on 2024-09-27 09:44_

---

_Comment by @kakkoyun on 2024-09-27 09:45_

<img src="https://media3.giphy.com/media/NtGCjwhjdwJcEWNnxc/giphy.gif"/>

---
