```yaml
number: 22409
title: "[ty] Ensure the ty playground module is only ever loaded once"
type: pull_request
state: merged
author: RasmusNygren
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: fix-playground-bug
created_at: 2026-01-05T20:21:33Z
updated_at: 2026-01-06T09:52:02Z
url: https://github.com/astral-sh/ruff/pull/22409
synced_at: 2026-01-12T15:57:49Z
```

# [ty] Ensure the ty playground module is only ever loaded once

---

_@RasmusNygren_

## Summary

Fixes a bug where the ty_wasm module was loaded twice when running locally. Most noticeably causing duplicate files.

<img width="1112" height="332" alt="image" src="https://github.com/user-attachments/assets/e7024f32-3a8b-44a9-9624-4d6a3505ebf7" />

Looking at the git history it seems like we did exactly this previously to ensure we only started the playground once but it seems to have gotten lost along the way.

## Test Plan
Run the playground locally and ensure that the bug no longer exists.


---

_Comment by @MichaReiser on 2026-01-05 20:24_

Oh, so I broke this with the ESLint upgrade. I think ESLint will get mad at you for reading the `ref` during render.

---

_Comment by @RasmusNygren on 2026-01-05 20:28_

> Oh, so I broke this with the ESLint upgrade. I think ESLint will get mad at you for reading the `ref` during render.

Oh that's why it was changed, that makes a lot of sense. The other approach it is then :)

---

_Label `playground` added by @AlexWaygood on 2026-01-05 23:40_

---

_Label `ty` added by @AlexWaygood on 2026-01-05 23:40_

---

_Converted to draft by @MichaReiser on 2026-01-06 08:54_

---

_@RasmusNygren reviewed on 2026-01-06 08:58_

---

_Review comment by @RasmusNygren on `playground/ty/src/Playground.tsx`:33 on 2026-01-06 08:58_

Can't say that I like this but not running `restoreWorkspace` multiple times seems to be what's actually important to not get the weird behaviour and I'm not sure if there's a much better way

---

_Renamed from "[ty] Ensure the ty playground is only ever started once" to "[ty] Ensure the ty playground module is only ever loaded once" by @RasmusNygren on 2026-01-06 09:02_

---

_Marked ready for review by @RasmusNygren on 2026-01-06 09:08_

---

_@MichaReiser reviewed on 2026-01-06 09:12_

---

_Review comment by @MichaReiser on `playground/ty/src/Playground.tsx`:33 on 2026-01-06 09:12_

Agree, this seems bad. 

Could we dispatch a `reset` action (using `dispatchFiles`) instead? 

The alternative is we use `useRef` and suppress the lint as I think it's a false positive anyway, given that we write to it exactly once

https://react.dev/reference/react/useRef#avoiding-recreating-the-ref-contents


Or we could try if something as mentioned in the React docs both satisfy TypeScript and ESLint:

```js
function Video() {
  const playerRef = useRef(null);

  function getPlayer() {
    if (playerRef.current !== null) {
      return playerRef.current;
    }
    const player = new VideoPlayer();
    playerRef.current = player;
    return player;
  }

  // ...
```

---

_@RasmusNygren reviewed on 2026-01-06 09:16_

---

_Review comment by @RasmusNygren on `playground/ty/src/Playground.tsx`:33 on 2026-01-06 09:16_

Ye I'm starting to think this is one of those cases where there's probably not enough reason to add workarounds just to satisfy the linter. I think I would prefer useRef and suppress the lint

---

_@MichaReiser reviewed on 2026-01-06 09:19_

---

_Review comment by @MichaReiser on `playground/ty/src/Playground.tsx`:33 on 2026-01-06 09:19_

Makes sense to me. We can add a comment explaining why

---

_Review requested from @carljm by @RasmusNygren on 2026-01-06 09:38_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2026-01-06 09:38_

---

_Review requested from @sharkdp by @RasmusNygren on 2026-01-06 09:38_

---

_Review requested from @dcreager by @RasmusNygren on 2026-01-06 09:38_

---

_Converted to draft by @RasmusNygren on 2026-01-06 09:39_

---

_Comment by @astral-sh-bot[bot] on 2026-01-06 09:40_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @RasmusNygren on 2026-01-06 09:40_

Whoops, that's a poor rebase from my side, sorry for pinging your review :D Please ignore

---

_Review request for @dcreager removed by @AlexWaygood on 2026-01-06 09:44_

---

_Review request for @carljm removed by @AlexWaygood on 2026-01-06 09:44_

---

_Review request for @sharkdp removed by @AlexWaygood on 2026-01-06 09:44_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2026-01-06 09:44_

---

_Marked ready for review by @RasmusNygren on 2026-01-06 09:48_

---

_@MichaReiser approved on 2026-01-06 09:51_

Thank you, and sorry for the back-and-forth.

---

_Merged by @MichaReiser on 2026-01-06 09:52_

---

_Closed by @MichaReiser on 2026-01-06 09:52_

---
