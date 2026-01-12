```yaml
number: 21308
title: "[ty] Add Django ORM type checking test coverage and comparison analysis"
type: pull_request
state: closed
author: saada
labels:
  - ty
assignees: []
base: main
head: django-support
created_at: 2025-11-06T22:38:34Z
updated_at: 2025-11-07T14:53:44Z
url: https://github.com/astral-sh/ruff/pull/21308
synced_at: 2026-01-12T15:57:20Z
```

# [ty] Add Django ORM type checking test coverage and comparison analysis

---

_@saada_

## Summary

This PR lays the **foundational test coverage** for Django ORM type checking in ty. It's intentionally ambitious and early-stage - documenting where ty stands today and establishing clear targets for future Django support.

**Note**: If the team feels this is too early or premature, happy to close it! The main goal is to at least get comprehensive test coverage down that documents the baseline and provides a roadmap for Django support.

## What's in this PR

### MDTest Coverage (6 test files)
- **basic_model.md** - Basic Model class detection and instance creation
- **queryset_manager.md** - QuerySet and Manager generic typing  
- **field_descriptors.md** - Django Field descriptor protocol tests
- **manager_synthesis.md** - Manager auto-synthesis and Self type tests
- **issue_1018.md** - Specific test for astral-sh/ty#1018 (id, objects, field access)
- **README.md** - Overview of Django support implementation phases

### Comparison Documentation
New `django_testing/` directory containing:
- **README.md** - Comprehensive comparison vs mypy/pyright with test results
- **django_comparison_test.py** - Real Django code for testing type checkers
- **mypy.ini & test_settings.py** - Configuration for running comparisons

## Test Format

Every test case includes a 4-point comparison showing:
1. **Current ty (no changes)**: What ty outputs today without any Rust changes
2. **Future ty (after fix)**: What ty should output after implementing fixes
3. **mypy + django-stubs**: What mypy reports with django-stubs plugin
4. **pyright + django-types**: What pyright reports with django-types

Example:
```python
reveal_type(article.title)  # revealed: Unknown | str
# 1. Current ty (no changes): Unknown | str
# 2. Future ty (after fix): str
# 3. mypy + django-stubs: str
# 4. pyright + django-types: str
```

This format makes it crystal clear what the baseline is, what the target should be, and how ty compares to the competition.

## Key Discoveries Documented

1. **Descriptor Protocol Partially Works!** - ty's descriptor protocol implementation returns `Unknown | str` instead of just `str`. This shows partial success - the descriptor protocol is working, but needs better type narrowing.

2. **Self Type Not Supported** - `Manager[Self]` fails with error "Expected `Model`, found `typing.Self`". This requires implementing PEP 673 Self type support.

3. **Two Distinct Issues**:
   - Unknown unions in field access (type narrowing problem)
   - Self type resolution (requires PEP 673 implementation)

## Why This Matters

Django is one of the most popular Python web frameworks. Having comprehensive test coverage:
- Establishes a baseline for measuring progress
- Documents exactly what needs to be implemented
- Provides a comparison benchmark against mypy and pyright
- Makes it easier for future contributors to pick up Django support work

## Test Status

✅ All 6 Django mdtest tests pass  
✅ Pre-commit checks pass (cargo fmt, clippy)  
✅ Tests accurately document current ty behavior  
✅ No Rust code changes - purely documentation/tests

## Implementation Phases (Future Work)

This PR establishes the test foundation for a phased Django support implementation:

- **Phase 0** (✅ Current): Basic Model class detection
- **Phase 1** (✅ Current): Generic QuerySet/Manager with manual annotations
- **Phase 2** (🔜 Next): Self type support, Manager auto-synthesis
- **Phase 3** (🔜 Future): Field descriptor improvements, nullable fields
- **Phase 4** (🔜 Future): QuerySet annotations, aggregations

## Related Issues

- Addresses https://github.com/astral-sh/ty/issues/291 (Django ORM type checking support)
- Addresses https://github.com/astral-sh/ty/issues/1018 (Django model attributes not recognized)

Both issues describe the same core problem: ty doesn't recognize Django's dynamically synthesized attributes (`.objects`, `.id`, field access). This PR documents the current state and establishes a path forward.

## Open Questions for Maintainers

1. **Timing**: Is it too early to add comprehensive Django test coverage? Happy to close if so, but wanted to at least get the foundation down.

2. **Test format**: Does the 4-point comparison format make sense? It's meant to show baseline → target → competition comparison clearly.

3. **Scope**: Should we split this into multiple PRs (e.g., tests-only first, then comparison docs)?

4. **Priority**: Where does Django support fit in ty's roadmap? This helps us prioritize which phases to tackle first.

Really excited about the potential here - Django support could be a major differentiator for ty! 🚀

---

_Review requested from @carljm by @saada on 2025-11-06 22:38_

---

_Review requested from @AlexWaygood by @saada on 2025-11-06 22:38_

---

_Review requested from @sharkdp by @saada on 2025-11-06 22:38_

---

_Review requested from @dcreager by @saada on 2025-11-06 22:38_

---

_Renamed from "Add Django ORM type checking test coverage and comparison analysis" to "[ty] Add Django ORM type checking test coverage and comparison analysis" by @AlexWaygood on 2025-11-06 22:42_

---

_Label `ty` added by @AlexWaygood on 2025-11-06 22:42_

---

_Comment by @sharkdp on 2025-11-07 14:53_

Thank you for your contribution. We are planning to add dedicated Django support at some point, but it's not on our short-term roadmap, so having extended tests in this area is probably just going to increase maintenance burden. We can always come back to this later, but I'd like to close this for now.

---

_Closed by @sharkdp on 2025-11-07 14:53_

---
