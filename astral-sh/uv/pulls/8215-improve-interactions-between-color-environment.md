```yaml
number: 8215
title: Improve interactions between color environment variables and CLI options
type: pull_request
state: merged
author: Aditya-PS-05
labels: []
assignees: []
merged: true
base: tracking/050
head: bug/8173-force-color
created_at: 2024-10-15T13:11:05Z
updated_at: 2024-11-08T05:03:45Z
url: https://github.com/astral-sh/uv/pull/8215
synced_at: 2026-01-12T16:08:12Z
```

# Improve interactions between color environment variables and CLI options

---

_@Aditya-PS-05_

closes #8173 

---

_Renamed from "new commit" to "disable force pull" by @Aditya-PS-05 on 2024-10-15 13:11_

---

_Renamed from "disable force pull" to "coloring by precedence" by @Aditya-PS-05 on 2024-10-15 13:12_

---

_Assigned to @charliermarsh by @zanieb on 2024-10-15 13:50_

---

_@charliermarsh reviewed on 2024-10-15 13:58_

---

_Review comment by @charliermarsh on `crates/uv/src/settings.rs`:79 on 2024-10-15 13:58_

I think this will always be true, right? We might need to make `color: Option<ColorChoice>` to distinguish the none case.

---

_@Aditya-PS-05 reviewed on 2024-10-15 14:02_

---

_Review comment by @Aditya-PS-05 on `crates/uv/src/settings.rs`:79 on 2024-10-15 14:02_

I thaught, if we have passed `--color` then it doesn't depend what is the value as it will always be used. 

I thought if 
```
error: invalid value 'none' for '--color <COLOR_CHOICE>'
  [possible values: auto, always, never]

For more information, try '--help'.
```
If only three values are defined. then no need to show support for `none`

---

_@Aditya-PS-05 reviewed on 2024-10-15 14:08_

---

_Review comment by @Aditya-PS-05 on `crates/uv/src/settings.rs`:79 on 2024-10-15 14:08_

Or this is a possibly new issue?

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:79 on 2024-10-15 14:29_

This value will always be populated because Clap fills in the default regardless of whether or not the user explicitly passed a value.

---

_@zanieb reviewed on 2024-10-15 14:30_

---

_@zanieb reviewed on 2024-10-15 14:30_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:79 on 2024-10-15 14:30_

I tried to bring this up at https://github.com/astral-sh/uv/pull/8183#discussion_r1799934233 but I think you misunderstood

---

_@Aditya-PS-05 reviewed on 2024-10-15 14:54_

---

_Review comment by @Aditya-PS-05 on `crates/uv/src/settings.rs`:79 on 2024-10-15 14:54_

Understood. 

---

_@Aditya-PS-05 reviewed on 2024-10-15 15:26_

---

_Review comment by @Aditya-PS-05 on `crates/uv/src/settings.rs`:79 on 2024-10-15 15:26_

Made 
```
pub color: Option<ColorChoice>,
``` 
```
if let Some(color_choice) = args.color {
                // If `--color` is passed explicitly, use its value.
                color_choice
            }
```

Everything is working as expected. 


---

_Comment by @charliermarsh on 2024-10-15 18:52_

My inclination is to make this part of 0.5.0 even though it's arguably a bug fix \cc @zanieb 

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-10-15 18:52_

---

_Comment by @zanieb on 2024-10-15 18:58_

Fine with me

---

_Renamed from "coloring by precedence" to "Improve interactions between color environment variables and CLI options" by @zanieb on 2024-10-21 17:04_

---

_Merged by @zanieb on 2024-10-21 17:04_

---

_Closed by @zanieb on 2024-10-21 17:04_

---

_Branch deleted on 2024-11-08 05:03_

---
