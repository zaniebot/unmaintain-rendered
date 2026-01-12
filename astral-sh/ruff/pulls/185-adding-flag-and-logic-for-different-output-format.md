```yaml
number: 185
title: Adding flag and logic for different output format
type: pull_request
state: merged
author: HallerPatrick
labels: []
assignees: []
merged: true
base: main
head: struct_output
created_at: 2022-09-14T12:18:51Z
updated_at: 2022-09-14T18:40:16Z
url: https://github.com/astral-sh/ruff/pull/185
synced_at: 2026-01-12T05:48:45Z
```

# Adding flag and logic for different output format

---

_Pull request opened by @HallerPatrick on 2022-09-14 12:18_

PR for #181, WIP

---

_Review comment by @charliermarsh on `src/main.rs`:63 on 2022-09-14 12:45_

Is `value_name` here necessary? If so, what's it enforcing?

---

_Review comment by @charliermarsh on `src/main.rs`:145 on 2022-09-14 12:46_

This looks like a merge / rebase issue -- do you mind restoring the `let mut settings = Settings::from_paths(&cli.files);` line, and removing lines 143 - 145?

---

_@charliermarsh requested changes on 2022-09-14 12:48_

---

_Comment by @charliermarsh on 2022-09-14 12:51_

Code generally looks good, thanks! Can you paste a screenshot or snippet of what the JSON output looks like? Separately, do you know what it takes to integrate this kind of JSON output into VSCode?

I do think it'd be good to support a custom message template as described in #181 but it doesn't have to be tackled here and is entirely compatible with this change.


---

_Review comment by @Stranger6667 on `src/printer.rs`:26 on 2022-09-14 12:56_

I'd suggest relaxing it a bit:

```suggestion
    pub fn write_once(&mut self, messages: &[Message]) -> Result<()> {
```

This way, it doesn't imply a heap allocation from the caller

---

_@HallerPatrick reviewed on 2022-09-14 13:05_

---

_Review comment by @HallerPatrick on `src/main.rs`:63 on 2022-09-14 13:05_

It is not. Removed it! 

---

_Review comment by @HallerPatrick on `src/main.rs`:145 on 2022-09-14 13:06_

Ah true. I restored it

---

_@HallerPatrick reviewed on 2022-09-14 13:06_

---

_@Stranger6667 reviewed on 2022-09-14 13:07_

---

_Comment by @HallerPatrick on 2022-09-14 13:13_

> Code generally looks good, thanks! Can you paste a screenshot or snippet of what the JSON output looks like? Separately, do you know what it takes to integrate this kind of JSON output into VSCode?
> 
> I do think it'd be good to support a custom message template as described in #181 but it doesn't have to be tackled here and is entirely compatible with this change.


Here part of the output (a list of all serialised messages): 

<img width="695" alt="Screenshot 2022-09-14 at 15 10 07" src="https://user-images.githubusercontent.com/22773355/190163021-8b01eb42-0d8e-4248-9dbd-ef7f970e7b29.png">

I am not a heavy VSCode user, but I will look into it :)



---

_@HallerPatrick reviewed on 2022-09-14 13:18_

---

_Review comment by @HallerPatrick on `src/printer.rs`:26 on 2022-09-14 13:18_

Made changes accordingly will latest commit

---

_Merged by @charliermarsh on 2022-09-14 14:43_

---

_Closed by @charliermarsh on 2022-09-14 14:43_

---

_Comment by @HallerPatrick on 2022-09-14 18:12_

@charliermarsh I just tested everything. But this PR introduced a bug with the `--watch` flag due to some (at least for me) unknown reasons. The latest commit fixed them. Could you please revert them or merge the newest version?

---

_Comment by @charliermarsh on 2022-09-14 18:20_

Sure, I just reverted.

---

_Comment by @charliermarsh on 2022-09-14 18:20_

Is this current version merge-able?

---

_Comment by @HallerPatrick on 2022-09-14 18:40_

I created a new PR. From there, I think, it is easier to merge from. The new one is mergeable

---
