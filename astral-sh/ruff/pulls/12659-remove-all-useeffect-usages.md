```yaml
number: 12659
title: "Remove all `useEffect` usages"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
assignees: []
merged: true
base: main
head: reduce-use-effect-usage
created_at: 2024-08-04T09:23:33Z
updated_at: 2024-08-08T11:16:40Z
url: https://github.com/astral-sh/ruff/pull/12659
synced_at: 2026-01-12T15:55:41Z
```

# Remove all `useEffect` usages

---

_@MichaReiser_

## Summary

The primary usage of effects is to interact with an external system. We don't have this. That's why this PR replaces all
`useEffect` usages with better suited hooks or removes them alltogether. See https://react.dev/learn/you-might-not-need-an-effect

## Test Plan

* Made changes
* Reload the editor
* Verified that changes are persisted
* Changed the theme to dark mode and reloaded the editor. Theme was persisted

I wasn't able to test sharing. The wrangler server goes up to 100% CPU usage whenever I hit share with or without my changes... 


---

_Review requested from @charliermarsh by @MichaReiser on 2024-08-04 09:23_

---

_Label `internal` added by @MichaReiser on 2024-08-04 09:23_

---

_@charliermarsh reviewed on 2024-08-05 00:29_

---

_Review comment by @charliermarsh on `playground/.gitignore`:132 on 2024-08-05 00:29_

Extra newline here.

---

_@charliermarsh reviewed on 2024-08-05 00:30_

---

_Review comment by @charliermarsh on `playground/src/main.tsx`:23 on 2024-08-05 00:30_

Doesn't this mean that nothing on the page will render until after the data fetch?

---

_Review comment by @dhruvmanila on `playground/src/main.tsx`:9 on 2024-08-05 03:43_

```suggestion
import init, { Workspace } from "./pkg/ruff_wasm";
```

---

_@dhruvmanila reviewed on 2024-08-05 03:43_

---

_@MichaReiser reviewed on 2024-08-05 06:05_

---

_Review comment by @MichaReiser on `playground/src/main.tsx`:23 on 2024-08-05 06:05_

Yes, that's correct. But arguably, this is the more "correct" behavior in the sense that showing anything is kind of meaningless and risks the user starting to change the code only so that it gets overridden once the response comes in. 

I think the "proper" solution would be to split our skeleton:

* The skeleton renders the header, left, and right navigation. It embeds the editor area
* Use a context to store the source and revision
* Fetch the code in the editor area
* Wrap the editor area in a suspense boundary

But that's a larger refactor than what I want to do right now.

---

_Label `playground` added by @MichaReiser on 2024-08-05 06:45_

---

_Label `internal` removed by @MichaReiser on 2024-08-05 06:45_

---

_@charliermarsh reviewed on 2024-08-05 13:03_

---

_Review comment by @charliermarsh on `playground/src/main.tsx`:23 on 2024-08-05 13:03_

But, on main, we don't even show the editor pane until the source is loaded, right?

---

_@MichaReiser reviewed on 2024-08-05 13:11_

---

_Review comment by @MichaReiser on `playground/src/Editor/Editor.tsx`:103 on 2024-08-05 13:11_

@charliermarsh I think we showed an empty editor and the `initAsync` that loads the wasm and restores the python code raced against the user making any changes :laughing: 

---

_@MichaReiser reviewed on 2024-08-05 13:11_

---

_Review comment by @MichaReiser on `playground/src/main.tsx`:23 on 2024-08-05 13:11_

We did show the editor. See https://github.com/astral-sh/ruff/pull/12659/files#r1704097956

Isn't that changing this is your main concern?

---

_@charliermarsh reviewed on 2024-08-05 13:13_

---

_Review comment by @charliermarsh on `playground/src/main.tsx`:23 on 2024-08-05 13:13_

I thought we showed the chrome (header) but not the editable panes in the editor, just based on reading the code where we have that `source ?` ternary in the editor body.

---

_Comment by @charliermarsh on 2024-08-05 13:13_

I (or someone) should probably test sharing before we merge this.

---

_Review comment by @MichaReiser on `playground/src/main.tsx`:23 on 2024-08-05 13:14_

ah yeah, that's correct. 

---

_@MichaReiser reviewed on 2024-08-05 13:14_

---

_@charliermarsh approved on 2024-08-05 13:17_

I do think we should test sharing before merging. I can try later today.

---

_@charliermarsh reviewed on 2024-08-05 13:18_

---

_Review comment by @charliermarsh on `playground/src/main.tsx`:23 on 2024-08-05 13:18_

I defer to you, I don't want to get in your way.

---

_Review comment by @MichaReiser on `playground/src/Editor/Chrome.tsx`:30 on 2024-08-05 13:59_

Too bad that `React.use` is experimental because we could then store the promise and have an inner component that is wrapped in a `Suspense` boundary and `use`s the promise. For now, this is a hacky version that doesn't show a loading indicator

---

_Review comment by @MichaReiser on `playground/src/main.tsx`:23 on 2024-08-05 14:00_

I extracted a `Chrome` component. It would have been nice if [`React.use` ](https://react.dev/reference/react/use) weren't experimental :(

---

_@MichaReiser reviewed on 2024-08-05 14:02_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-08-06 07:53_

---

_Comment by @MichaReiser on 2024-08-08 11:16_

Okay, I had to accept or decline the telemetry option, then force kill all wrangler process to get it working. But sharing works 

---

_Merged by @MichaReiser on 2024-08-08 11:16_

---

_Closed by @MichaReiser on 2024-08-08 11:16_

---

_Branch deleted on 2024-08-08 11:16_

---
