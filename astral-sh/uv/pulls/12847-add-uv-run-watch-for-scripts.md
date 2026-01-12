```yaml
number: 12847
title: "Add `uv run --watch` for scripts"
type: pull_request
state: closed
author: thejchap
labels: []
assignees: []
draft: true
base: main
head: feature/run-watch
created_at: 2025-04-11T22:28:12Z
updated_at: 2025-07-31T19:45:47Z
url: https://github.com/astral-sh/uv/pull/12847
synced_at: 2026-01-12T16:10:24Z
```

# Add `uv run --watch` for scripts

---

_@thejchap_

## Summary
(Closes #9652)

Adds watch mode for running scripts, enabled via a `--watch` flag

### Implementation
- Currently, on file change, `SIGTERM`, or `SIGINT`, the child interpreter process is always killed via [SIGKILL](https://docs.rs/tokio/latest/tokio/process/struct.Child.html#method.kill). Then, is restarted in the case of file change. The watcher process always exists with exit code 0.
  - This omits quite a bit of the complexity in [run_to_completion](https://github.com/thejchap/uv/blob/feature/run-watch/crates/uv/src/commands/run.rs#L10-L10) - it seems like there might be a larger refactor lurking there if we want to overload that function to support restarting the script after its killed
  - This also ignores any exit code that comes back from the command after it finishes executing - ie if you kill the watcher process while the command is running, its just going to pull the plug on the command and return "ok" rather than sending a signal to the command, then letting the command finish and propagating that exit code back up.
- ~Clearing the screen~
  - ~Bun [does this by default](https://bun.sh/docs/runtime/hot) but also provides a flag to turn it off - should we do the same?~
  - Added a flag
- ~Debounce duration~
  - ~Bun looks like it has no debounce - need to look at some other examples in the wild to see what other libraries do - picked 100ms for now very arbitrarily~
  - Looks like Red Knot [uses 10](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_project/src/watch/watcher.rs#L69), stole this

## Test Plan
TODO on real tests, MVP E2E here:

https://github.com/user-attachments/assets/d576f227-7461-4e4f-b3ed-f81957bc685e



---

_Comment by @alanmoyano on 2025-04-11 23:23_

It can handle re-running a script after crashing? A nice use case for my would be spawning back a flask server after fixing the bug that made it crash. In any case, looking great!

---

_Review requested from @Gankra by @Gankra on 2025-04-14 13:52_

---

_Assigned to @Gankra by @zanieb on 2025-04-15 20:19_

---

_Comment by @slhck on 2025-04-16 07:09_

This would be a great addition!

Note that `tsx`, for example, in [its `--watch` mode](https://tsx.is/watch-mode), also has an option to disable clearing the screen, which is useful for capturing logs from older runs of the same script — and `node` has [the same functionality](https://nodejs.org/docs/latest/api/cli.html#--watch-preserve-output).

---

_Comment by @thejchap on 2025-04-25 13:53_

@Gankra if you want to let me know if this is on the right track i can make some more progress here in the next few days

---

_Comment by @BoynChan on 2025-06-04 08:01_

That's cool job, looking forward next move!

---

_Comment by @thejchap on 2025-06-22 01:42_

closing this - happy to resume work on it after some feedback

---

_Closed by @thejchap on 2025-06-22 01:42_

---

_Comment by @blakerutledge-g on 2025-07-31 19:45_

any update on this? would love to use this right now ha

---
