```yaml
number: 22787
title: "isort: Add `sort-by-qualified-name` option"
type: pull_request
state: open
author: stephenfin
labels: []
assignees: []
base: main
head: hacking-import-order
created_at: 2026-01-21T12:29:23Z
updated_at: 2026-01-21T18:27:10Z
url: https://github.com/astral-sh/ruff/pull/22787
synced_at: 2026-01-21T19:05:04Z
```

# isort: Add `sort-by-qualified-name` option

---

_@stephenfin_

## Summary

Currently, the isort rules sort imports purely based on module path. For example, the following would pass isort with the default configuration:

    from foo import baz
    from foo.bar import wow

since `foo` < `foo.bar`. However, the [hacking][1] plugin for flake8 provides rule `H306`, which expects [that imports are sorted by their fully qualified name][2]. Therefore it would expect the following:

    from foo.bar import wow
    from foo import baz

since `foo.bar.wow` < `foo.baz`.

Add support for this functionality via a new `sort-by-qualified-name` knob which defaults to `false`. As discussed inline, this is superseded by the `length-sort` option to preserve existing behavior.

## Test Plan

Unit tests included. Change ran against a subset of the many OpenStack projects using ruff for linting.

[1]: https://opendev.org/openstack/hacking/
[2]: https://opendev.org/openstack/hacking/src/commit/235d79d911d55abb0978fcf5ac92e193ebf9efa5/hacking/checks/imports.py#L81-L106


---

_Renamed from "isort: Add sort-by-qualified-name option" to "isort: Add `sort-by-qualified-name` option" by @stephenfin on 2026-01-21 12:32_

---
