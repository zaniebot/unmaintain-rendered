```yaml
number: 14702
title: Add oxipng pre commit hook
type: pull_request
state: closed
author: UnknownPlatypus
labels: []
assignees: []
base: main
head: add-oxipng-pre-commit-hook
created_at: 2024-12-01T19:46:48Z
updated_at: 2024-12-08T17:05:21Z
url: https://github.com/astral-sh/ruff/pull/14702
synced_at: 2026-01-12T15:55:48Z
```

# Add oxipng pre commit hook

---

_@UnknownPlatypus_

## Summary
Compress png assets using [oxipng](github.com/shssoichiro/oxipng) and add a pre-commit hook to make this process automatic.
See https://github.com/shssoichiro/oxipng?tab=readme-ov-file#git-integration-via-pre-commit

Was trying this out on some repos because I had great success using this at work and on some personal projects.
Feel free to close if it's not appropriate.

## Test Plan
I ran `pre-commit run oxipng --all-files` which led to some great compresions.
```shel
1967 bytes (60.29% smaller): playground/public/apple-touch-icon.png
12685 bytes (45.85% smaller): crates/ruff_server/assets/nativeServer.png
40751 bytes (1.27% smaller): crates/red_knot/docs/tracing-flamegraph.png
755 bytes (62.77% smaller): playground/public/favicon-32x32.png
2132 bytes (44.85% smaller): assets/png/Astral.png
376 bytes (49.93% smaller): docs/assets/ruff-favicon.png
537 bytes (54.65% smaller): playground/public/favicon-16x16.png
377626 bytes (34.53% smaller): playground/public/Ruff.png
```


---

_Review requested from @carljm by @UnknownPlatypus on 2024-12-01 19:46_

---

_Review requested from @MichaReiser by @UnknownPlatypus on 2024-12-01 19:46_

---

_Review requested from @AlexWaygood by @UnknownPlatypus on 2024-12-01 19:46_

---

_Review requested from @sharkdp by @UnknownPlatypus on 2024-12-01 19:46_

---

_Comment by @github-actions[bot] on 2024-12-02 00:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-12-02 07:53_

Thanks for the PR. 

It's not entirely clear to me what the benefit of optimizing the PNGs are because most of them aren't part of the website. 

For the playground, I'd prefer integration with vite (the bundler) instead of running some command at commit time because it allows us to commit the high-quality image and only optimize it for the web when creating the bundle.

For the docs, I'd prefer an integration into mkdocs (if it does not already do so). 

Considering the above, I don't think this is the right approach because it doesn't distinguish between use case and prevents us from committing full-res pictures.

---

_Closed by @MichaReiser on 2024-12-02 07:53_

---

_Branch deleted on 2024-12-08 17:05_

---
