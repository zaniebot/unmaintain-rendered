---
number: 1973
title: "`check` and `check --watch` have different results"
type: issue
state: closed
author: fosskers
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-12-17T02:29:36Z
updated_at: 2026-01-09T09:34:32Z
url: https://github.com/astral-sh/ty/issues/1973
synced_at: 2026-01-10T01:48:23Z
---

# `check` and `check --watch` have different results

---

_Issue opened by @fosskers on 2025-12-17 02:29_

### Summary

Hi there, thanks for `ty`.

In experimenting with `--watch` today I noticed that it reports fewer results than a normal `ty check` does. I'm running it like:
```
uv run ty check FOO/ --watch
```
Currently, this reports no errors under `FOO/`. However without `--watch` it reports 9.

### Version

0.0.4

---

_Comment by @zanieb on 2025-12-17 03:25_

Can you share a minimal example we can use to reproduce?

---

_Comment by @fosskers on 2025-12-17 04:51_

Unfortunately the code is not open source.

---

_Label `bug` added by @AlexWaygood on 2025-12-17 07:35_

---

_Label `needs-mre` added by @AlexWaygood on 2025-12-17 07:35_

---

_Comment by @MichaReiser on 2025-12-17 07:58_

Would you be able to share the logs when running `ty check FOO -v` and `ty check FOO -v --watch`?

---

_Comment by @zanieb on 2025-12-17 14:13_

Can you produce it with a minimized example of the code then?

---

_Added to milestone `Stable` by @carljm on 2025-12-17 23:30_

---

_Comment by @fosskers on 2025-12-19 01:22_

Caught in the wild: just `uv run ty check` yields 25 results, while `--watch` only detects 16.

I'm not seeing anything particularly incriminating in the output of `-v`, other than some files taking more than 100ms to check. Note though that `-v` with `--watch` doesn't really work, as `--watch` clears the screen.

---

_Comment by @fosskers on 2025-12-19 01:23_

`--watch` just found 20 diagnostics, yet I changed absolutely nothing. Non-determinism? Race conditions in parallel checks? Timeouts?

I'm on `0.0.4` at the moment.

Update: It's back down to 16 ... now back up to 21.

---

_Comment by @MichaReiser on 2025-12-19 07:10_

Do you see the difference on the first run of `ty check --watch` or are the diagnostics only different after you made changes (and check ran multiple times?)

---

_Comment by @fosskers on 2025-12-19 07:18_

If I activate `ty check --watch`, kill it, then reactivate, then repeat, yes I see it alternating between 16 results and 20 results, with no other code changes from me.

---

_Comment by @MichaReiser on 2025-12-19 07:22_

With kill and reactivate, you mean you stop ty, then re-run the command? 



---

_Comment by @fosskers on 2025-12-19 07:25_

Correct, via `Ctrl-C`.

---

_Comment by @MichaReiser on 2025-12-19 07:26_

Very interesting:

* ty doesn't use any persistent caching. Each run is a fresh run
* The `--watch` and non-watch use the exact same code paths. The only difference is that `ty check` (without watch) exits the main loop immediately after check completed once

---

_Comment by @fosskers on 2025-12-19 07:31_

Tried to kill my editor and run `ty` in isolation to see if my LSP was somehow `touch`ing files and confusing `--watch`, but no, same observed behaviour.

---

_Comment by @fosskers on 2025-12-19 07:52_

```rust
    fn watch(mut self, db: &mut ProjectDatabase) -> Result<ExitStatus> {
        tracing::debug!("Starting watch mode");
        let sender = self.sender.clone();
        let watcher = watch::directory_watcher(move |event| {
            sender.send(MainLoopMessage::ApplyChanges(event)).unwrap();
        })?;

        self.watcher = Some(ProjectWatcher::new(watcher, db));
        self.run(db)?;

        Ok(ExitStatus::Success)
    }
```
Could multiple events be fighting to apply changes at the same time?

---

_Comment by @MichaReiser on 2025-12-19 09:33_

> Could multiple events be fighting to apply changes at the same time?

That should only be an issue for subsequent runs of the watch, but not the initial run. Also, updates abort all pending checks and ty then restarts.

---

_Comment by @MichaReiser on 2025-12-19 11:14_

I tried to reproduce the issue by running ty on `discord.py` in watch mode but it always reported the same number of diagnostics. 

But I put up https://github.com/astral-sh/ruff/pull/22078, which should hopefully make this easier to debug. 

By chance, do any of the flaky diagnostics say something about `Divergent`? There are some rare cases where we experience non-determinism in ty and maybe they only happen to show up in watch moide (for reasons?)

---

_Comment by @MichaReiser on 2026-01-09 09:34_

I'll close this as I haven't been able to reproduce, which makes the issue non-actionable for us. Feel free to comment on this issue if you're still experiencing this problem or open a new issue.

---

_Closed by @MichaReiser on 2026-01-09 09:34_

---
