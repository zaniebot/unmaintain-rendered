```yaml
number: 13266
title: "ProgressReporter: align progress bars by largest name length"
type: pull_request
state: merged
author: eduardorittner
labels:
  - cli
assignees: []
merged: true
base: main
head: align-prog
created_at: 2025-05-02T14:06:35Z
updated_at: 2025-05-15T07:32:37Z
url: https://github.com/astral-sh/uv/pull/13266
synced_at: 2026-01-12T16:10:37Z
```

# ProgressReporter: align progress bars by largest name length

---

_@eduardorittner_

## Summary

Related to https://github.com/astral-sh/uv/issues/12492

This change makes all progress bars vertically aligned. This is still a WIP and so is not complete, in the current design I store `max_len` in `BarState` and update it on every `on_request_start`, however this is problematic since order matters, and if the largest name is not sent first, the alignment is not complete. To mitigate this we'd probably have to update all previous bars by "iterating" through the `bars` field in `BarState` and update all request bars.

Below is an image of what happens when the largest name (`nvidia-cusparselt-cu12`) is not the first (in this case, it was the second to last).

![2025-05-02T10:56:54-03:00](https://github.com/user-attachments/assets/ac6f2205-5f30-4fe3-a2c3-f980e36b7cf7)


## Test Plan

There are currently no tests, and I'm not sure how to design them since from what I gather the `uv_snapshot` facilities record the final output, not the intermediate stages.


---

_Label `cli` added by @zanieb on 2025-05-09 14:53_

---

_Review requested from @konstin by @zanieb on 2025-05-09 14:53_

---

_Assigned to @konstin by @zanieb on 2025-05-09 14:53_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/reporters.rs`:69 on 2025-05-09 17:35_

You should be able to clean this up a little bit.
```suggestion
            headers: 0,
            sizes: Vec::default(),
            bars: FxHashMap::default(),
            size: FxHashMap::default(),
            id: 0,
```

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/reporters.rs`:189 on 2025-05-09 17:38_

You can write this as a nice one-liner (and get rid of the method).
```suggestion
        self.max_len = cmp::max(self.max_len, len);
```

---

_@ibraheemdev reviewed on 2025-05-09 17:39_

Just left a couple of nits. I think you're right that we need to update previous bars to keep things aligned, but that shouldn't be too hard.

It might be a little odd if the padding changes for all bars dynamically, maybe we should at least increase the default from 10?

---

_@ibraheemdev reviewed on 2025-05-09 17:41_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/reporters.rs`:245 on 2025-05-09 17:41_

Right here is where you should be able to iterate through the `multi_progress` and realign everything.

---

_Review comment by @konstin on `--`:1 on 2025-05-13 18:46_

This file looks accidental

---

_@konstin reviewed on 2025-05-13 18:46_

---

_Comment by @konstin on 2025-05-13 18:48_

Hi, and sorry for the late review. Could you change the code so that when we have a new longest progress bar, it updates all running progress bars to the new template with the new length (as you suggested), to avoid a pattern like below?

![image](https://github.com/user-attachments/assets/7abd500e-f59d-49d6-b646-fd3645a2cfca)


---

_Comment by @eduardorittner on 2025-05-14 13:20_

Thank you for all the comments and suggestions! I'm having a busy month with school and work but will work on this when I have time 😁

---

_@eduardorittner reviewed on 2025-05-14 20:18_

---

_Review comment by @eduardorittner on `crates/uv/src/commands/reporters.rs`:245 on 2025-05-14 20:18_

This is what I'm having a hard time with, indicatif's `MultiProgress` only has an `insert` method, with no way of iterating or getting/setting specific values inside it. Which means I should probably be iterating through a field in `BarState`? But I'm not sure which one, since IDs are only stored in maps, not vectors. Maybe I could figure out from `BarState.headers` and `BarState.id`, how many headers are downloads (`BarState.id - BarState.headers`?) and then iterate from `BarState.id - BarState.headers` to `BarState.id` accessing the relevant bar in `BarState.bars`.

---

_Comment by @eduardorittner on 2025-05-14 20:23_

I mentioned this in the PR description, but do you have any ideas on how to test this feature? Since we care about the intermediate state while downloads are happening (dynamically adjusting bar length) not only the final one, should we have several snapshots of the output? How can we simulate downloads without actually hitting the network?

---

_Comment by @eduardorittner on 2025-05-14 20:23_

> It might be a little odd if the padding changes for all bars dynamically, maybe we should at least increase the default from 10?

Changed it to 20, let me know what you think!

---

_@eduardorittner reviewed on 2025-05-14 20:27_

---

_Review comment by @eduardorittner on `--`:1 on 2025-05-14 20:27_

Whoops, removed

---

_@eduardorittner reviewed on 2025-05-14 20:28_

---

_Review comment by @eduardorittner on `crates/uv/src/commands/reporters.rs`:245 on 2025-05-14 20:28_

I just did the dumb thing and iterated through `0..BarState.id` and it *seems* to work, I'm not sure if there's a situation where it breaks or does something unexpected.

---

_Comment by @eduardorittner on 2025-05-14 20:29_

Another question I have is if I should write similar logic in `on_build_start`? If so, any tips on what/how to build with uv? I've only ever downloaded packages with it so I'm unsure

---

_Marked ready for review by @eduardorittner on 2025-05-14 20:29_

---

_Comment by @eduardorittner on 2025-05-14 20:36_

> Hi, and sorry for the late review. Could you change the code so that when we have a new longest progress bar, it updates all running progress bars to the new template with the new length (as you suggested), to avoid a pattern like below?
> 
> ![image](https://private-user-images.githubusercontent.com/6826232/443354257-7abd500e-f59d-49d6-b646-fd3645a2cfca.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDcyNTM0ODQsIm5iZiI6MTc0NzI1MzE4NCwicGF0aCI6Ii82ODI2MjMyLzQ0MzM1NDI1Ny03YWJkNTAwZS1mNTlkLTQ5ZDYtYjY0Ni1mZDM2NDVhMmNmY2EucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI1MDUxNCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNTA1MTRUMjAwNjI0WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9ZTMyMGMwMDhmNzY2YzZkYTUzOTdkNmU2OGUwNmY4NGM5NzNlM2Q2MTI1YmI4MTM2MDczYzFlMmNkOTI4YzQ4MyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QifQ.N1c7jpuxjfJtZUYZyQvCU9QlTDX-J4ufPSCDxDZuS54)

Also which package are you downloading here? I wanted to test this with a lot of packages but `torch` is only pulling like 10 dependencies

---

_Comment by @konstin on 2025-05-15 06:31_

> I mentioned this in the PR description, but do you have any ideas on how to test this feature? Since we care about the intermediate state while downloads are happening (dynamically adjusting bar length) not only the final one, should we have several snapshots of the output? How can we simulate downloads without actually hitting the network?

We test this manually part manually, we don't have snapshots for non-static text.

> Another question I have is if I should write similar logic in `on_build_start`? If so, any tips on what/how to build with uv? I've only ever downloaded packages with it so I'm unsure

The builds use a spinner instead of a progress bar (we can't tell how long they will take), so this is only relevant for `on_*_progress` bars.

> Also which package are you downloading here? I wanted to test this with a lot of packages but `torch` is only pulling like 10 dependencies


I'm using

```
uv venv -p 3.11 && cargo run pip install -r scripts/requirements/transformers-extras.in
```

sometimes with `--no-cache`.

---

_Comment by @konstin on 2025-05-15 07:12_

I've updated the code to use an enum to only apply this logic to numeric progress while skipping progress spinners, and I've rebased for the merge conflict with main. 

[Screencast from 2025-05-15 09-11-13.webm](https://github.com/user-attachments/assets/8901a989-567c-42fe-961c-640cfc08528c)


---

_Merged by @konstin on 2025-05-15 07:32_

---

_Closed by @konstin on 2025-05-15 07:32_

---
