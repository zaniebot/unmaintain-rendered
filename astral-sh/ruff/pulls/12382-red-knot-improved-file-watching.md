```yaml
number: 12382
title: "[red-knot] Improved file watching "
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: watch
created_at: 2024-07-18T16:10:29Z
updated_at: 2024-07-23T06:19:01Z
url: https://github.com/astral-sh/ruff/pull/12382
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] Improved file watching 

---

_Pull request opened by @MichaReiser on 2024-07-18 16:10_

## Summary

This PR improves Red Knot's file watching by handling all events emitted by notify (except the ones we aren't interested in) and adding tests. 

This PR also makes the following changes:

* It adds a new `--watch` option to the CLI. Red knot does a one pass analysis if the option is set to `false` (default).  
* It splits the `by_path` in `Files` into two: One for system paths and another for vendored paths. It helps to reduce the search space when syncing the state of all files (because we can easily skip all vendored paths)

### Why aren't we using [`notify-debouncer-mini`](https://github.com/notify-rs/notify/tree/main/notify-debouncer-mini) or [`notify-debouncer-full`](https://github.com/notify-rs/notify/tree/main/notify-debouncer-full)?

`notify-debouncer-mini` is too minimal. It aggregates all events to a [single event type](https://github.com/notify-rs/notify/blob/main/notify-types/src/debouncer_mini.rs), indicating that *something* happened, but we would no longer know what happened. 

`notify-debouncer-full` is very promising, and it even provides functionality to match the rename events `MovedFrom` and `MovedTo`, which allows Red Knot to do less when a file or directory is moved. However, this functionality depends on the system's file handles, and the way it gets the file handles (at least on non-Unix and Android systems) is by recursively traversing every watched directory ([source](https://github.com/notify-rs/notify/blob/128bf6230c03d39dbb7f301ff7b20e594e34c3a2/notify-debouncer-full/src/cache.rs#L68-L83)). I don't think this is a good trade-off for us. This means Red Knot eagerly traverses all module search paths and the workspace root during initialization. This is in addition to Red Knot already walking all packages during startup, significantly delaying the time until Red Knot starts checking.

There's an option to disable the file-id caching by using `NoCache`. This would give us the event merging on UNIX (because it doesn't require the file cache and instead uses the `inotify` tracker) but not on all other platforms. While nice, I rather prefer consistent behavior between platforms and this implementation already optimizes the `MovedFrom` event if it is known that the path was a file (because we tracked it in `Files`). In which case we would end up doing more work than necessary.


So here we are. One does too much, and the other does too little 😆 

## Test Plan

See the added integration tests. 



---

_Comment by @github-actions[bot] on 2024-07-18 16:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_outlook.ipynb:13:1:1: Expected an expression
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_outlook.ipynb:13:1:1: Expected an expression
```

</p>
</details>




---

_Label `red-knot` added by @MichaReiser on 2024-07-19 09:05_

---

_Marked ready for review by @MichaReiser on 2024-07-19 09:56_

---

_Review requested from @carljm by @MichaReiser on 2024-07-19 09:56_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-19 09:56_

---

_@MichaReiser reviewed on 2024-07-19 10:01_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:138 on 2024-07-19 10:01_

We may want to optimize this in the future, but I have decided against it for now. I suspect that the cost of iterating all files is neglectable compared to the necessary sys-calls to read the file metadata.

We'll see once Red Knot is capable of analyzing real world projects.

---

_Review comment by @carljm on `crates/red_knot/src/watch/watcher.rs`:171 on 2024-07-20 02:39_

Is this a case where `Rescan` might make sense? Or is that too aggressive?

---

_Review comment by @carljm on `crates/red_knot/tests/file_watching.rs`:45 on 2024-07-20 02:42_

Is this unavoidable? What kind "catching up" is needed that can't be expressed explicitly via a method that waits for the appropriate conditions? Sleeping in tests for an arbitrary amount of time is never good; inevitably it ends up being too short on some system and leading to flakiness, and wastes test-running time on every other system.

---

_@carljm approved on 2024-07-20 02:45_

Looks great! Love all the tests, and although there's a lot of code here, the overall structure is pretty clear.

---

_@MichaReiser reviewed on 2024-07-20 07:08_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/file_watching.rs`:45 on 2024-07-20 07:08_

> Is this unavoidable?

I couldn't figure out an alternative. I hate it too. 

There are a couple of things involved:

* We have to wait for the platform-specific syscall (`inotify`, `fsevents`, ...) to inform us about the file change
* The syscall doesn't happen on the main thread. So we have to wait for the file watcher, then the debounce threads to wake up and handle the event

I considered using a spin-loop that waits until the first event comes in, but even that's not guaranteed and risks dead-locking. So we would need something more complicated that avoids dead-locks as well (another debounce implementation :laughing:)

I'm open to any suggestions.

---

_@MichaReiser reviewed on 2024-07-20 10:24_

---

_Review comment by @MichaReiser on `crates/red_knot/src/watch/watcher.rs`:171 on 2024-07-20 10:24_

Possibly, not sure. I honestly don't understand when this is happening well enough yet. We may also have to propagate errors to `apply_changes` for it to decide what to do in the case of an error. 

For now, I prefer to leave it as is and wait until we can do some testing on real world projects (e.g. by navigating through individual commits) and hopefully that allows us to trigger some of the errors and then decide what we should do about them.

---

_@carljm reviewed on 2024-07-22 21:46_

---

_Review comment by @carljm on `crates/red_knot/tests/file_watching.rs`:45 on 2024-07-22 21:46_

Yes, this is tricky. Although not perfect, I think better than a fixed wait would be to have the watcher thread set a condvar (maybe only in tests) when it has debounced some events, and do a `wait_timeout` on that condvar here. The `wait_timeout` ensures that in the failure case where no event ever arrives we don't deadlock, but since in the success case we don't have to wait the full timeout, we can set it sufficiently high that it should never timeout just due to a slow system.

But I'm also OK with just adding a TODO here for now, and delaying improvement until such future time when we are specifically investing in reducing test-running time or test flakiness.

---

_@MichaReiser reviewed on 2024-07-23 06:18_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/file_watching.rs`:45 on 2024-07-23 06:18_

I don't think we need any condvar for this. We can try to read from the `changes_receiver` (using the `select!` macro) and use a high timeout if no message is received in that time. But this is still not perfect. What if we receive a first batch of changes but then miss the last change message because it was a bit slower? 

I don't think a TODO is warranted because I don't plan to do anything about it unless the tests prove to be flaky. I also rather prefer the 10ms slow down than any complex system that only is prone to other kind of errors and is further away to what we use in production.  I'm not worried about the test performance. Ten ms isn't long enough to significantly slow down tests.  

---

_Merged by @MichaReiser on 2024-07-23 06:18_

---

_Closed by @MichaReiser on 2024-07-23 06:18_

---

_Branch deleted on 2024-07-23 06:19_

---
