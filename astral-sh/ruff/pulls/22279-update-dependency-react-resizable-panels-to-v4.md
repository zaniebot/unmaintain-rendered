```yaml
number: 22279
title: Update dependency react-resizable-panels to v4
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/react-resizable-panels-4.x
created_at: 2025-12-29T16:16:45Z
updated_at: 2026-01-01T15:37:37Z
url: https://github.com/astral-sh/ruff/pull/22279
synced_at: 2026-01-12T15:57:45Z
```

# Update dependency react-resizable-panels to v4

---

_@renovate_

This PR contains the following updates:

| Package | Change | [Age](https://docs.renovatebot.com/merge-confidence/) | [Confidence](https://docs.renovatebot.com/merge-confidence/) |
|---|---|---|---|
| [react-resizable-panels](http://react-resizable-panels.now.sh/) ([source](https://redirect.github.com/bvaughn/react-resizable-panels)) | [`^3.0.0` -> `^4.0.0`](https://renovatebot.com/diffs/npm/react-resizable-panels/3.0.6/4.0.15) | ![age](https://developer.mend.io/api/mc/badges/age/npm/react-resizable-panels/4.0.15?slim=true) | ![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/react-resizable-panels/3.0.6/4.0.15?slim=true) |

---

### Release Notes

<details>
<summary>bvaughn/react-resizable-panels (react-resizable-panels)</summary>

### [`v4.0.15`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#4015)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.14...4.0.15)

- [556](https://redirect.github.com/bvaughn/react-resizable-panels/pull/556): Ignore `defaultLayout` when keys don't match Panel ids

### [`v4.0.14`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#4014)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.13...4.0.14)

- [555](https://redirect.github.com/bvaughn/react-resizable-panels/pull/555): Allow resizable panels to be rendered into a different Window (e.g. popup or frame) by accessing globals through `element.ownerDocument.defaultView`

### [`v4.0.13`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#4013)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.12...4.0.13)

- `useDefaultLayout`: Deprecated `groupId` param in favor of `id` to avoid confusion; (there is no actual requirement for the Group to have a matching id)

### [`v4.0.12`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#4012)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.11...4.0.12)

- [552](https://redirect.github.com/bvaughn/react-resizable-panels/pull/552): `useDefaultLayout` now debounces calls to `storage.setItem` by 150ms

```ts
// To opt out of this change
useDefaultLayout({
  debounceSaveMs: 0,
  groupId: "test-group-id",
  storage: localStorage,
})
```

> \[!NOTE]
> Some may consider this a breaking change, considering the default value is 150ms rather than 0ms. I think in practice this should only impact unit tests which can be easily fixed by overriding the default (as shown above) or by using fake timers.
>
> Changes like this are often judgement calls, but I think on balance it's better to correct my initial oversight of not debouncing this by default.

### [`v4.0.11`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#4011)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.10...4.0.11)

- [8604491](https://redirect.github.com/bvaughn/react-resizable-panels/commit/8604491): Fix edge case bug with panel constraints not being properly invalidated after resize

### [`v4.0.10`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#4010)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.9...4.0.10)

- [#&#8203;551](https://redirect.github.com/bvaughn/react-resizable-panels/pull/551): Expand fixed-size element support

### [`v4.0.9`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#409)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.8...4.0.9)

- [#&#8203;542](https://redirect.github.com/bvaughn/react-resizable-panels/pull/542): Clicks on higher `z-index` elements (e.g. modals) should not trigger separators behind them
- [#&#8203;547](https://redirect.github.com/bvaughn/react-resizable-panels/pull/547): Don't re-mount Group when `defaultLayout` or `disableCursor` props change
- [#&#8203;548](https://redirect.github.com/bvaughn/react-resizable-panels/pull/548): Bugfix: Gracefully handle `Panel` id changes
- [#&#8203;549](https://redirect.github.com/bvaughn/react-resizable-panels/pull/549): Improve DevX when Group within hidden DOM subtree; defer layout-change events

### [`v4.0.8`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#408)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.7...4.0.8)

- [#&#8203;541](https://redirect.github.com/bvaughn/react-resizable-panels/pull/541): Don't set invalid layouts when Group is hidden or has a width/height of 0
- [40d4356](https://redirect.github.com/bvaughn/react-resizable-panels/commit/40d4356): Gracefully handle invalid `defaultLayout` value

### [`v4.0.7`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#407)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.6...4.0.7)

- [f07bf00](https://redirect.github.com/bvaughn/react-resizable-panels/commit/f07bf00): Reset `pointer-event` styles after "pointerup" event

### [`v4.0.6`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#406)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.5...4.0.6)

- [0796644](https://redirect.github.com/bvaughn/react-resizable-panels/commit/0796644): Account for Flex gap when calculating pointer-move delta %

### [`v4.0.5`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#405)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.4...4.0.5)

- [#&#8203;535](https://redirect.github.com/bvaughn/react-resizable-panels/pull/535): Updated docs to make size and layout formats clearer

### [`v4.0.4`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#404)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.3...4.0.4)

- [#&#8203;534](https://redirect.github.com/bvaughn/react-resizable-panels/pull/534): Set focus on `Separator` on "pointerdown"
- [e08fe42](https://redirect.github.com/bvaughn/react-resizable-panels/commit/e08fe42195d8ace7e4e62205453be4a5245fefb9): Improve iOS/Safari resize UX

### [`v4.0.3`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#403)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.2...4.0.3)

- Fixed TS type for `defaultLayout` value returned from `useDefaultLayout`

### [`v4.0.2`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#402)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.1...4.0.2)

- Export `GroupImperativeHandle` and `PanelImperativeHandle` types.

### [`v4.0.1`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#4013)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/4.0.0...4.0.1)

- `useDefaultLayout`: Deprecated `groupId` param in favor of `id` to avoid confusion; (there is no actual requirement for the Group to have a matching id)

### [`v4.0.0`](https://redirect.github.com/bvaughn/react-resizable-panels/blob/HEAD/CHANGELOG.md#400)

[Compare Source](https://redirect.github.com/bvaughn/react-resizable-panels/compare/3.0.6...4.0.0)

Version 4 of react-resizable-panels offers more flexible size constraints– supporting units as pixels, percentages, REMs/EMs, and more. Support for server-rendering (including Server Components) has also been expanded.

#### Migrating from version 3 to 4

Refer to [the docs](https://react-resizable-panels.now.sh/) for a complete list of props and API methods. Below are some examples of migrating from version 3 to 4, but first a couple of potential questions:

<dl>
<dt>Q: Why'd you rename &lt;component&gt; or &lt;prop&gt;?</dt>
<dd>A: The most likely reason is that I think the new name more closely aligns with web standards like WAI-ARIA and CSS. For example, the <code>PanelResizeHandle</code> component was renamed to <code>Separator</code> to better align with the <a href="https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Roles/separator_role">ARIA "separator" role</a> and the <code>direction</code> prop was renamed to <code>orientation</code> to better align with the <a href="https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-orientation">ARIA <code>orientation</code> attribute </a>.</dd>
<dt>Q: Why'd you remove support for &lt;feature&gt;?</dt>
<dd>A: Probably because it wasn't used widely enough to justify the complexity required to maintain it. If it turns out that I'm mistaken, features can always be (re)added but it's more difficult to remove them.</dd>
<dt>Q: Were the <code>onCollapse</code> and <code>onExpand</code> event handlers removed?</dt>
<dd>A: Yes. Use the <code>onResize</code> event handler instead:

```ts
onResize={(size) => {
  // Either this
  const isCollapsed = size === collapsedSize;

  // Or this:
  panelRef.isCollapsed();
}}
```

</dd>
</dl>

##### Basic usage example

```tsx
// Version 3

import { PanelGroup, Panel, PanelResizeHandle } from "react-resizable-panels";

<PanelGroup direction="horizontal">
  <Panel defaultSize={30} minSize={20}>left</Panel>
  <PanelResizeHandle />
  <Panel defaultSize={30} minSize={20}>right</Panel>
</PanelGroup>

// Version 4

import { Group, Panel, Separator } from "react-resizable-panels";

<Group orientation="horizontal">
  <Panel defaultSize="30%" minSize="20%">left</Panel>
  <Separator />
  <Panel defaultSize="30%" minSize="20%">right</Panel>
</Group>
```

##### Persistent layouts using localStorage

```tsx
// Version 3

import { PanelGroup, Panel, PanelResizeHandle } from "react-resizable-panels";

<PanelGroup autoSaveId="unique-group-id" direction="horizontal">
  <Panel>left</Panel>
  <PanelResizeHandle />
  <Panel>right</Panel>
</PanelGroup>

// Version 4

import { Group, Panel, Separator, useDefaultLayout } from "react-resizable-panels";

const { defaultLayout, onLayoutChange } = useDefaultLayout({
  groupId: "unique-group-id",
  storage: localStorage
});

<Group defaultLayout={defaultLayout} onLayoutChange={onLayoutChange}>
  <Panel>left</Panel>
  <Separator />
  <Panel>right</Panel>
</Group>
```

> \[!NOTE]
> Refer to [the docs](https://react-resizable-panels.vercel.app/examples/persistent-layout) for examples of persistent layouts with server rendering and server components.

##### Conditional panels

```tsx
// Version 3

import { PanelGroup, Panel, PanelResizeHandle } from "react-resizable-panels";

<PanelGroup autoSaveId="unique-group-id" direction="horizontal">
   {showLeftPanel && (
     <>
       <Panel id="left" order={1}>left</Panel>
       <PanelResizeHandle />
     </>
   )}
   <Panel id="center" order={2}>center</Panel>
   {showRightPanel && (
     <>
       <PanelResizeHandle />
       <Panel id="right" order={3}>right</Panel>
     </>
   )}
</PanelGroup>

// Version 4

import { Group, Panel, Separator } from "react-resizable-panels";

<Group>
  {showLeftPanel && (
    <>
      <Panel id="left">left</Panel>
      <Separator />
    </>
  )}
  <Panel id="center">center</Panel>
  {showRightPanel && (
    <>
      <Separator />
      <Panel id="right">right</Panel>
    </>
  )}
</Group>
```

##### Imperative APIs

```tsx
// Version 3

import { PanelGroup, Panel, PanelResizeHandle } from "react-resizable-panels";
import type { ImperativePanelGroupHandle, ImperativePanelHandle }from "react-resizable-panels";

 const panelRef = useRef<ImperativePanelHandle>(null);
 const panelGroupRef = useRef<ImperativePanelGroupHandle>(null);

<PanelGroup direction="horizontal" ref={panelGroupRef}>
  <Panel ref={panelRef}>left</Panel>
  <PanelResizeHandle />
  <Panel>right</Panel>
</PanelGroup>

// Version 4

import { Group, Panel, Separator, useGroupRef, usePanelRef } from "react-resizable-panels";

const groupRef = useGroupRef();
const panelRef = usePanelRef();

<Group groupRef={groupRef} orientation="horizontal">
  <Panel panelRef={panelRef}>left</Panel>
  <Separator />
  <Panel>right</Panel>
</Group>
```

##### Disabling custom cursors

```tsx
// Version 3

import { disableGlobalCursorStyles } from "react-resizable-panels";

disableGlobalCursorStyles();

// Version 4

import { Group, Panel, Separator } from "react-resizable-panels";

<Group disableCursor />
```

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi41OS4wIiwidXBkYXRlZEluVmVyIjoiNDIuNTkuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-12-29 16:16_

---

_Comment by @MichaReiser on 2025-12-29 17:32_

Uff, this will be quiet some work

---

_Comment by @MichaReiser on 2025-12-29 17:41_

Looking great 🥹 
<img width="2192" height="656" alt="Screenshot 2025-12-29 at 18 41 32" src="https://github.com/user-attachments/assets/93cf2de1-b72c-4761-830e-220da999dc9b" />


---

_Comment by @silamon on 2025-12-29 20:13_

Vertical orientation now needs to set a height: https://github.com/bvaughn/react-resizable-panels/issues/543
When that's done, it will look like before. 

---

_Comment by @MichaReiser on 2025-12-30 07:26_

Thanks for pointing this out. I did set a high but there's now something odd going on where it results in added scrollbars on the main div if the secondary panel is a monaco editor. 

---

_Merged by @MichaReiser on 2025-12-30 10:04_

---

_Closed by @MichaReiser on 2025-12-30 10:04_

---

_Branch deleted on 2025-12-30 10:04_

---

_Comment by @AlexWaygood on 2026-01-01 15:28_

This change appears to have broken local builds of the playground for me. I now just get an entirely white screen when I open the local playground build in a web browser:

<img width="2700" height="1684" alt="image" src="https://github.com/user-attachments/assets/f010e660-e900-4932-8c3d-fe08c9ac005e" />

The regression bisects to this commit: if I checkout https://github.com/astral-sh/ruff/commit/3d35dbd334e7cf04fd3533c51d0ef1302f96bcd4 and build the playground locally, everything's fine

---

_Comment by @MichaReiser on 2026-01-01 15:34_

Did you run `npm install`? What error message do you see in the browser?

---

_Comment by @AlexWaygood on 2026-01-01 15:36_

The command I always run to build the playground locally is `cd playground && npm start --workspace ty-playground`. I see this error message in the browser:

```
Uncaught SyntaxError: The requested module 'http://localhost:5173/node_modules/.vite/deps/react-resizable-panels.js?v=03e9ad63' doesn't provide an export named: 'Separator' ResizeHandle.tsx:1:10
```

---

_Comment by @AlexWaygood on 2026-01-01 15:37_

Running `npm install` seems to have fixed it, thanks!

---
