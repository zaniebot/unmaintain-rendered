```yaml
number: 10611
title: Update dependency react-resizable-panels to v2
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/react-resizable-panels-2.x
created_at: 2024-03-26T11:01:09Z
updated_at: 2024-03-26T11:52:27Z
url: https://github.com/astral-sh/ruff/pull/10611
synced_at: 2026-01-12T15:55:32Z
```

# Update dependency react-resizable-panels to v2

---

_@renovate_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Change | Age | Adoption | Passing | Confidence |
|---|---|---|---|---|---|
| [react-resizable-panels](https://togithub.com/bvaughn/react-resizable-panels) | [`^0.0.54` -> `^2.0.0`](https://renovatebot.com/diffs/npm/react-resizable-panels/0.0.54/2.0.16) | [![age](https://developer.mend.io/api/mc/badges/age/npm/react-resizable-panels/2.0.16?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![adoption](https://developer.mend.io/api/mc/badges/adoption/npm/react-resizable-panels/2.0.16?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![passing](https://developer.mend.io/api/mc/badges/compatibility/npm/react-resizable-panels/0.0.54/2.0.16?slim=true)](https://docs.renovatebot.com/merge-confidence/) | [![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/react-resizable-panels/0.0.54/2.0.16?slim=true)](https://docs.renovatebot.com/merge-confidence/) |

---

### Release Notes

<details>
<summary>bvaughn/react-resizable-panels (react-resizable-panels)</summary>

### [`v2.0.16`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.16)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.15...2.0.16)

-   Replaced `.toPrecision()` with `.toFixed()` to avoid undesirable layout shift ([#&#8203;323](https://togithub.com/bvaughn/react-resizable-panels/issues/323))

### [`v2.0.15`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.15)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.14...2.0.15)

-   Better account for high-precision sizes with `onCollapse` and `onExpand` callbacks ([#&#8203;325](https://togithub.com/bvaughn/react-resizable-panels/issues/325))

### [`v2.0.14`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.14)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.13...2.0.14)

-   Better account for high-precision `collapsedSize` values ([#&#8203;325](https://togithub.com/bvaughn/react-resizable-panels/issues/325))

### [`v2.0.13`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.13)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.12...2.0.13)

-   Fix potential cycle in stacking-order logic for an unmounted node ([#&#8203;317](https://togithub.com/bvaughn/react-resizable-panels/issues/317))

### [`v2.0.12`](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.11...2.0.12)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.11...2.0.12)

### [`v2.0.11`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.11)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.10...2.0.11)

-   Fix resize handle cursor hit detection when when viewport is scrolled ([#&#8203;305](https://togithub.com/bvaughn/react-resizable-panels/issues/305))

### [`v2.0.10`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.10)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.9...2.0.10)

-   Fix conditional layout edge case ([#&#8203;309](https://togithub.com/bvaughn/react-resizable-panels/issues/309))

### [`v2.0.9`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.9)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.8...2.0.9)

-   Fix Flex stacking context bug ([#&#8203;301](https://togithub.com/bvaughn/react-resizable-panels/issues/301))
-   Fix case where pointer event listeners were sometimes added to the document unnecessarily

### [`v2.0.8`](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.7...2.0.8)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.7...2.0.8)

### [`v2.0.7`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.7)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.6...2.0.7)

-   Group default layouts use `toPrecision` to avoid small layout shifts due to floating point precision differences between initial server rendering and client hydration ([#&#8203;295](https://togithub.com/bvaughn/react-resizable-panels/issues/295))

### [`v2.0.6`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.6)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.5...2.0.6)

-   Replace `useLayoutEffect` usage with SSR-safe wrapper hook ([#&#8203;294](https://togithub.com/bvaughn/react-resizable-panels/issues/294))

### [`v2.0.5`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.5)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.4...2.0.5)

-   Resize handle hit detection considers [stacking context](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Understanding_z-index/Stacking_context) when determining hit detection ([#&#8203;291](https://togithub.com/bvaughn/react-resizable-panels/issues/291))

### [`v2.0.4`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.4)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.3...2.0.4)

-   Fixed `PanelResizeHandle` `onDragging` prop to only be called for the handle being dragged ([#&#8203;289](https://togithub.com/bvaughn/react-resizable-panels/issues/289))

### [`v2.0.3`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.3)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.2...2.0.3)

-   Fix resize handle onDragging callback ([#&#8203;278](https://togithub.com/bvaughn/react-resizable-panels/issues/278))

### [`v2.0.2`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.2)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.1...2.0.2)

-   Fixed an issue where size might not be re-initialized correctly after a panel was hidden by the `unstable_Activity` (previously "Offscreen") API.

### [`v2.0.1`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.1)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/2.0.0...2.0.1)

-   Fixed a regression introduced in 2.0.0 that caused React `onClick` and `onMouseUp` handlers not to fire.

### [`v2.0.0`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/2.0.0)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/1.0.10...2.0.0)

-   Support resizing multiple (intersecting) panels at once ([#&#8203;274](https://togithub.com/bvaughn/react-resizable-panels/issues/274))
    This behavior can be customized using a new `hitAreaMargins` prop; defaults to a 15 pixel margin for *coarse* inputs and a 5 pixel margin for *fine* inputs.

### [`v1.0.10`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/1.0.10)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/1.0.9...1.0.10)

-   Fixed edge case constraints check bug that could cause a collapsed panel to re-expand unnecessarily ([#&#8203;273](https://togithub.com/bvaughn/react-resizable-panels/issues/273))

### [`v1.0.9`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/1.0.9)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/1.0.8...1.0.9)

-   DOM util methods scope param defaults to `document` ([#&#8203;262](https://togithub.com/bvaughn/react-resizable-panels/issues/262))
-   Updating a `Panel`'s pixel constraints will trigger revalidation of the `Panel`'s size ([#&#8203;266](https://togithub.com/bvaughn/react-resizable-panels/issues/266))

### [`v1.0.8`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/1.0.8)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/1.0.7...1.0.8)

-   Update component signature to declare `ReactElement` return type (rather than `ReactNode`) ([#&#8203;256](https://togithub.com/bvaughn/react-resizable-panels/issues/256))
-   Update `Panel` dev warning to avoid warning when `defaultSize === collapsedSize` for collapsible panels ([#&#8203;257](https://togithub.com/bvaughn/react-resizable-panels/issues/257))
-   Support shadow dom by removing direct references to / dependencies on the root `document` ([#&#8203;204](https://togithub.com/bvaughn/react-resizable-panels/issues/204))

### [`v1.0.7`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/1.0.7)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/1.0.6...1.0.7)

-   Narrow `tagName` prop to only allow `HTMLElement` names (rather than the broader `Element` type) ([#&#8203;251](https://togithub.com/bvaughn/react-resizable-panels/issues/251))

### [`v1.0.6`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/1.0.6)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/1.0.5...1.0.6)

-   Export internal DOM helper methods.

### [`v1.0.5`](https://togithub.com/bvaughn/react-resizable-panels/compare/1.0.4...1.0.5)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/1.0.4...1.0.5)

### [`v1.0.4`](https://togithub.com/bvaughn/react-resizable-panels/compare/1.0.3...1.0.4)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/1.0.3...1.0.4)

### [`v1.0.3`](https://togithub.com/bvaughn/react-resizable-panels/compare/1.0.2...1.0.3)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/1.0.2...1.0.3)

### [`v1.0.2`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/1.0.2)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/1.0.1...1.0.2)

-   Change local storage key for persisted sizes to avoid restoring pixel-based sizes ([#&#8203;233](https://togithub.com/bvaughn/react-resizable-panels/issues/233))

### [`v1.0.1`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/1.0.1)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/1.0.0...1.0.1)

-   Small bug fix to guard against saving an incorrect panel layout to local storage

### [`v1.0.0`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/1.0.0)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/0.0.63...1.0.0)

-   Remove support for pixel-based Panel constraints; (props like `defaultSizePercentage` should now be `defaultSize`)
-   Replaced `dataAttributes` prop with `...rest` prop that supports all HTML attributes

### [`v0.0.63`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/0.0.63)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/0.0.62...0.0.63)

-   Change default (not-yet-registered) Panel flex-grow style from 0 to 1

### [`v0.0.62`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/0.0.62)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/0.0.61...0.0.62)

-   Edge case expand/collapse invalid size guard ([#&#8203;220](https://togithub.com/bvaughn/react-resizable-panels/issues/220))

### [`v0.0.61`](https://togithub.com/bvaughn/react-resizable-panels/compare/0.0.60...0.0.61)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/0.0.60...0.0.61)

### [`v0.0.60`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/0.0.60)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/0.0.59...0.0.60)

-   Better support imperative API usage from mount effects.
-   Better support strict effects mode.
-   Better checks not to call `onResize` or `onLayout` more than once.

### [`v0.0.59`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/0.0.59)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/0.0.58...0.0.59)

-   Support imperative panel API usage on-mount.
-   Made PanelGroup bailout condition smarter (don't bailout for empty groups unless pixel constraints are used).
-   Improved window splitter compatibility by better handling "Enter" key.

### [`v0.0.58`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/0.0.58)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/0.0.57...0.0.58)

-   Change group layout to more thoroughly distribute resize delta to support more flexible group size configurations.
-   Add data attribute support to `Panel`, `PanelGroup`, and `PanelResizeHandle`.
-   Update API documentation to reflect changed imperative API method names.
-   `PanelOnResize` TypeScript def updated to reflect that previous size param is `undefined` the first time it is called.

### [`v0.0.57`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/0.0.57)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/0.0.56...0.0.57)

-   [#&#8203;207](https://togithub.com/bvaughn/react-resizable-panels/pull/207): Fix DEV conditional error that broke data attributes (and selectors).

### [`v0.0.56`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/0.0.56)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/0.0.55...0.0.56)

Support a mix of percentage and pixel based units at the `Panel` level:

```jsx
<Panel defaultSizePixels={100} minSizePercentage={20} maxSizePercentage={50} />
```

> **Note**: Pixel units require the use of a `ResizeObserver` to validate. Percentage based units are recommended when possible.

##### Example migrating panels with percentage units

<table>
  <thead>
    <tr>
      <th>v55</th>
      <th>v56</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <pre lang="jsx">
&lt;Panel
  defaultSize={25}
  minSize={10}
  maxSize={50}
/&gt;
        </pre>
      </td>
      <td>
        <pre lang="jsx">
&lt;Panel
  defaultSizePercentage={25}
  minSizePercentage={10}
  maxSizePercentage={50}
/&gt;
        </pre>
      </td>
    </tr>
  </tbody>
</table>

##### Example migrating panels with pixel units

<table>
  <thead>
    <tr>
      <th>v55</th>
      <th>v56</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <pre lang="jsx">
&lt;PanelGroup
  direction="horizontal"
  units="pixels"
&gt;
  &lt;Panel minSize={100} maxSize={200} /&gt;
  &lt;PanelResizeHandle /&gt;
  &lt;Panel /&gt;
&lt;/PanelGroup&gt;
        </pre>
      </td>
      <td>
        <pre lang="jsx">
&lt;PanelGroup direction="horizontal"&gt;
  &lt;Panel
    minSizePixels={100}
    maxSizePixels={200}
  /&gt;
  &lt;PanelResizeHandle /&gt;
  &lt;Panel /&gt;
&lt;/PanelGroup&gt;
        </pre>
      </td>
    </tr>
  </tbody>
</table>

For a complete list of supported properties and example usage, refer to the docs.

### [`v0.0.55`](https://togithub.com/bvaughn/react-resizable-panels/releases/tag/0.0.55)

[Compare Source](https://togithub.com/bvaughn/react-resizable-panels/compare/0.0.54...0.0.55)

-   New `units` prop added to `PanelGroup` to support pixel-based panel size constraints.

This prop defaults to "percentage" but can be set to "pixels" for static, pixel based layout constraints.

This can be used to add enable pixel-based min/max and default size values, e.g.:

```jsx
<PanelGroup direction="horizontal" units="pixels">
  {/* Will be constrained to 100-200 pixels (assuming group is large enough to permit this) */}
  <Panel minSize={100} maxSize={200} />
  <PanelResizeHandle />
  <Panel />
  <PanelResizeHandle />
  <Panel />
</PanelGroup>
```

Imperative API methods are also able to work with either pixels or percentages now. They default to whatever units the group has been configured to use, but can be overridden with an additional, optional parameter, e.g.

```ts
panelRef.resize(100, "pixels");
panelGroupRef.setLayout([25, 50, 25], "percentages");

// Works for getters too, e.g.
const percentage = panelRef.getSize("percentages");
const pixels = panelRef.getSize("pixels");

const layout = panelGroupRef.getLayout("pixels");
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

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4yNjkuMiIsInVwZGF0ZWRJblZlciI6IjM3LjI2OS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiJ9-->


---

_Label `internal` added by @renovate[bot] on 2024-03-26 11:01_

---

_Merged by @MichaReiser on 2024-03-26 11:52_

---

_Closed by @MichaReiser on 2024-03-26 11:52_

---

_Branch deleted on 2024-03-26 11:52_

---
