```yaml
number: 9476
title: "Promote `lint.` settings over top-level settings"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
assignees: []
merged: true
base: release/0.2.0
head: promote-lint-options-over-top-level-settings
created_at: 2024-01-11T19:09:50Z
updated_at: 2024-01-29T17:42:32Z
url: https://github.com/astral-sh/ruff/pull/9476
synced_at: 2026-01-10T22:57:09Z
```

# Promote `lint.` settings over top-level settings

---

_Pull request opened by @MichaReiser on 2024-01-11 19:09_

## Summary

This PR marks the top-level lint settings as deprecated (schema only) and updates the website to document the options under the lint section (removes the top-level settings). 

I had to make changes to the documentation generation to now support 3-level deep options

* The documentation now shows the entire name for sub-sub-sections (e.g. `lint.pydocstyle` instead of `pydocstyle`) because sub-sub-sections are rendered at the same level as sub-sections. 
* The anchor link now contains the entire parent chain: `lint.pycodestyle.xxx` instead of `pycodestyle.xxx`. 
* The anchor link now use `_` instead of `-` as a separator to disambiguate between the subsection `lint.pydocstyle` and option `lint-pydocstyle`

I feel undecided about one open question: Should we use the full setting name in READMEs or only the name? For example. Should we write `lint.ignore` or `ignore`? Or a more extreme example. Should we write max-complexity` or `lint.mccabe.max-complexity`. We currently use a mix of both:

* The tutorial mainly uses the short names
* Rules use the full name (until today without the `lint` prefix)

I changed the tutorial to use long names in most positions but it didn't always feel right.  

## Considerations

One downside of removing the top-level settings from the website is that Ruff's documentation isn't versioned. Users using an older version of ruff that only supports the global settings or use the global options might be confused why the options are missing. 

A possible solution is adding aliasing support to our documentation generation, although that might be tricky.

IMO, removing is fine because the `lint` section has been supported since [v0.0.292](https://github.com/astral-sh/ruff/releases/tag/v0.0.292), which feels like a very long time ago. 

## Next steps

Emit deprecation messages when a configuration uses any deprecated top-level settings.

## Test Plan

* I built the documentation locally and verified that the lint options are only listed once under the `lint` section (which helps discoverability). 
* I read through the documentation and clicked through the setting options to verify that the anchor links work
* I reviewed the schema changes and it marks the top level lint settings as deprecated.


![image](https://github.com/astral-sh/ruff/assets/1203881/ea5d8bc5-e525-45bb-98ff-57af032b5513)


> **Danger**: Only merge before the 0.2.0 release


---

_Label `breaking` added by @MichaReiser on 2024-01-11 19:11_

---

_Label `configuration` added by @MichaReiser on 2024-01-11 19:11_

---

_Label `breaking` removed by @MichaReiser on 2024-01-11 19:11_

---

_Comment by @codspeed-hq[bot] on 2024-01-19 14:03_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/promote-lint-options-over-top-level-settings)

### Merging #9476 will **not alter performance**

<sub>:warning: No base runs were found</sub>

<sub>Falling back to comparing <code>promote-lint-options-over-top-level-settings</code> (6a200c0) with <code>main</code> (50122d2)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@MichaReiser reviewed on 2024-01-19 14:07_

---

_Review comment by @MichaReiser on `docs/formatter.md`:333 on 2024-01-19 14:07_

An example where we don't use full qualified setting names.

---

_Review requested from @charliermarsh by @MichaReiser on 2024-01-19 14:12_

---

_Review requested from @zanieb by @MichaReiser on 2024-01-19 14:12_

---

_Added to milestone `v0.2.0` by @MichaReiser on 2024-01-19 14:13_

---

_Marked ready for review by @MichaReiser on 2024-01-19 14:15_

---

_Comment by @github-actions[bot] on 2024-01-19 14:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh reviewed on 2024-01-22 04:34_

---

_Review comment by @charliermarsh on `docs/formatter.md`:333 on 2024-01-22 04:34_

This seems ok to me.

---

_@charliermarsh reviewed on 2024-01-22 04:35_

---

_Review comment by @charliermarsh on `docs/linter.md`:28 on 2024-01-22 04:35_

All the changes in here look reasonable to me.

---

_@charliermarsh reviewed on 2024-01-22 04:41_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/function_call_in_argument_default.rs`:64 on 2024-01-22 04:41_

Did you verify manually that these still work, when rendered in the rule documentation?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/never_union.rs`:33 on 2024-01-22 04:46_

:)

---

_@charliermarsh reviewed on 2024-01-22 04:46_

---

_@charliermarsh reviewed on 2024-01-22 04:47_

---

_Review comment by @charliermarsh on `crates/ruff_dev/src/generate_options.rs`:175 on 2024-01-22 04:47_

This all got so much simpler, where did the simplification come from?

---

_@charliermarsh approved on 2024-01-22 04:48_

The changes you made (to the docs, etc.) generally look good to me. It's a shame that existing anchor links won't work any more, but it seems really hard to avoid. Thanks for cleaning this up.

---

_@MichaReiser reviewed on 2024-01-22 07:29_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/function_call_in_argument_default.rs`:64 on 2024-01-22 07:29_

I verified some of them ;)

---

_@MichaReiser reviewed on 2024-01-22 07:31_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_options.rs`:175 on 2024-01-22 07:31_

By writing simpler code :laughing: 

I realised that the only difference between the two is whether we add the `tool.ruff` prefix or not. The fact that `parents` is now an iter made it more obvious to me that we could use `iter.join` over manually constructing the string. 

---

_Comment by @MichaReiser on 2024-01-22 07:33_

> The changes you made (to the docs, etc.) generally look good to me. It's a shame that existing anchor links won't work any more, but it seems really hard to avoid. Thanks for cleaning this up.

We could add backwards-compatible anchors. We could add backwards compatible ids if we want to keep anchor links from external websites working (I hope I managed to update all internal references). 

---

_Merged by @MichaReiser on 2024-01-29 17:42_

---

_Closed by @MichaReiser on 2024-01-29 17:42_

---

_Branch deleted on 2024-01-29 17:42_

---
