```yaml
number: 22740
title: "Stabilize `blank_line_before_decorated_class_in_stub`"
type: pull_request
state: open
author: dylwil3
labels:
  - breaking
  - formatter
assignees: []
base: 2026-style
head: stabilize-blank_line_before_decorated_class_in_stub
created_at: 2026-01-19T20:37:32Z
updated_at: 2026-01-19T20:49:08Z
url: https://github.com/astral-sh/ruff/pull/22740
synced_at: 2026-01-19T21:33:07Z
```

# Stabilize `blank_line_before_decorated_class_in_stub`

---

_@dylwil3_

Don't get excited! Opening PRs for all preview styles to decide.


---

_Review requested from @MichaReiser by @dylwil3 on 2026-01-19 20:37_

---

_Label `breaking` added by @dylwil3 on 2026-01-19 20:37_

---

_Label `formatter` added by @dylwil3 on 2026-01-19 20:37_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 20:49_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+1 -0 lines in 1 file in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 lines across 1 file)</summary>
<p>

<a href='https://github.com/python/typeshed/blob/4992a813c160e037712b0793cd0488275cb3d195/stubs/channels/channels/consumer.pyi#L39'>stubs/channels/channels/consumer.pyi~L39</a>
```diff
 # Accepts any ASGI message dict with a required "type" key (str),
 # but allows additional arbitrary keys for flexibility.
 def get_handler_name(message: dict[str, Any]) -> str: ...
+
 @type_check_only
 class _ASGIApplicationProtocol(Protocol):
     consumer_class: AsyncConsumer
```

</p>
</details>

### Formatter (preview)
✅ ecosystem check detected no format changes.





---
