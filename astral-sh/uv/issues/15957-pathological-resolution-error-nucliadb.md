```yaml
number: 15957
title: "Pathological resolution error: nucliadb"
type: issue
state: open
author: konstin
labels:
  - error messages
assignees: []
created_at: 2025-09-19T22:37:22Z
updated_at: 2025-09-21T03:47:17Z
url: https://github.com/astral-sh/uv/issues/15957
synced_at: 2026-01-12T16:02:20Z
```

# Pathological resolution error: nucliadb

---

_@konstin_

The `uv pip compile` error when trying to resolve `nucliadb>=3` with `--no-build` on Python 3.11 is 135k lines (universal) or 133k lines (standard). This is a pathological case, as the are ~4500 release of nucliadb since. 

We can use this as a worst case as base for improving bad resolver error traces.

```
echo "nucliadb>=3" | uv pip compile --universal - --no-build -p 3.13
```

or

```
echo "nucliadb>=3" | uv pip compile - --no-build -p 3.13
```

Excerpt from the universal resolution error:

```
  × No solution found when resolving dependencies for split (markers:
  │ python_full_version >= '3.14'):
  ╰─▶ Because only the following versions of nucliadb-telemetry[all] are
      available:
          nucliadb-telemetry[all]<=3.0.0.post414
          nucliadb-telemetry[all]>=3.0.0.post415
      and nucliadb-telemetry[all]>=3.0.0.post414,<=3.2.0.post513
      depends on opentelemetry-proto==1.21.0, we can conclude that
      nucliadb-telemetry[all]>=3.0.0.post414,<3.0.0.post415 depends on
      opentelemetry-proto==1.21.0.
      And because nucliadb-telemetry>=5.0.0.post931 depends on pydantic>=2.6
      and pydantic>=2.7, we can conclude that opentelemetry-proto!=1.21.0,
      pydantic<2.6, all of:
          nucliadb-telemetry[all]>=3.0.0.post414,<3.0.0.post415
          nucliadb-telemetry[all]>=4.0.0.post515
       are incompatible.
      And because nucliadb-telemetry[all]>=3.0.0.post415,<=3.2.0.post513
      depends on opentelemetry-proto==1.21.0 and opentelemetry-proto==1.21.0
      depends on protobuf>=3.19,<5.0, we can conclude that all of:
          protobuf<3.19
          protobuf>=5.0
      , pydantic<2.6, all of:
          nucliadb-telemetry[all]>=3.0.0.post414,<=3.0.3.post460
          nucliadb-telemetry[all]>=4.0.0.post515
       are incompatible.
      And because only the following versions of nucliadb-telemetry[all] are
      available:
          nucliadb-telemetry[all]<=3.0.3.post460
          nucliadb-telemetry[all]==3.0.3.post478
          nucliadb-telemetry[all]==3.0.3.post479
          nucliadb-telemetry[all]==3.0.3.post480
          nucliadb-telemetry[all]==3.0.3.post481
          nucliadb-telemetry[all]==3.0.3.post482
          nucliadb-telemetry[all]==3.0.3.post483
          nucliadb-telemetry[all]==3.0.3.post484
          nucliadb-telemetry[all]==3.0.3.post485
          nucliadb-telemetry[all]==3.0.3.post486
          nucliadb-telemetry[all]==3.0.3.post487
          nucliadb-telemetry[all]==3.0.3.post488
          nucliadb-telemetry[all]==3.0.3.post489
          nucliadb-telemetry[all]==3.0.3.post490
          nucliadb-telemetry[all]==3.0.3.post491
          nucliadb-telemetry[all]==3.1.0.post492
          nucliadb-telemetry[all]==3.1.0.post493
          nucliadb-telemetry[all]==3.1.0.post494
          nucliadb-telemetry[all]==3.1.0.post495
          nucliadb-telemetry[all]==3.1.0.post496
          nucliadb-telemetry[all]==3.1.0.post497
          nucliadb-telemetry[all]==3.1.0.post498
          nucliadb-telemetry[all]==3.1.0.post499
[...]
      Because only the following versions of nucliadb-utils[cache] are available:
          nucliadb-utils[cache]<=6.8.1.post4957
          nucliadb-utils[cache]>=6.8.1.post4961
      and nucliadb-utils>=6.8.1.post4957 depends on nats-py[nkeys]>=2.6.0, we can conclude that nucliadb-utils[cache]>=6.8.1.post4957,<6.8.1.post4961 depends
      on nats-py[nkeys]>=2.6.0.
      And because nucliadb-utils==6.8.1.post4961 depends on nats-py[nkeys]>=2.6.0, we can conclude that nucliadb-utils[cache]>=6.8.1.post4957 depends on
      nats-py[nkeys]>=2.6.0. (436)

      Because only the following versions of nats-py[nkeys] are available:
          nats-py[nkeys]<=2.6.0
          nats-py[nkeys]==2.7.0
          nats-py[nkeys]==2.7.2
          nats-py[nkeys]==2.8.0
          nats-py[nkeys]==2.9.0
          nats-py[nkeys]==2.9.1
          nats-py[nkeys]==2.10.0
          nats-py[nkeys]==2.11.0
      and nats-py[nkeys]>=2.6.0 has no usable wheels, we can conclude that nats-py[nkeys]>=2.6.0 cannot be used.
      And because we know from (436) that nucliadb-utils[cache]>=6.8.1.post4957 depends on nats-py[nkeys]>=2.6.0, we can conclude that
      nucliadb-utils[cache]>=6.8.1.post4957 cannot be used.
      And because only nucliadb-utils[cache]<=6.8.1.post4961 is available and nucliadb==6.8.1.post4957 depends on nucliadb-utils[cache]>=6.8.1.post4957, we
      can conclude that nucliadb==6.8.1.post4957 cannot be used.
      And because we know from (435) that nucliadb>=3,<6.8.1.post4957 cannot be used, we can conclude that nucliadb>=3,<6.8.1.post4961 cannot be used. (437)

      Because only the following versions of nats-py[nkeys] are available:
          nats-py[nkeys]<=2.6.0
          nats-py[nkeys]==2.7.0
          nats-py[nkeys]==2.7.2
          nats-py[nkeys]==2.8.0
          nats-py[nkeys]==2.9.0
          nats-py[nkeys]==2.9.1
          nats-py[nkeys]==2.10.0
          nats-py[nkeys]==2.11.0
      and nats-py[nkeys]>=2.6.0 has no usable wheels, we can conclude that nats-py[nkeys]>=2.6.0 cannot be used.
      And because nucliadb-utils==6.8.1.post4961 depends on nats-py[nkeys]>=2.6.0, we can conclude that nucliadb-utils==6.8.1.post4961 cannot be used.
      And because only nucliadb-utils[cache]<=6.8.1.post4961 is available and nucliadb==6.8.1.post4961 depends on nucliadb-utils[cache]>=6.8.1.post4961, we
      can conclude that nucliadb==6.8.1.post4961 cannot be used.
      And because we know from (437) that nucliadb>=3,<6.8.1.post4961 cannot be used, we can conclude that nucliadb>=3 cannot be used.
      And because you require nucliadb>=3, we can conclude that your requirements are unsatisfiable.

      hint: Pre-releases are available for `grpcio-tools` in the requested range (e.g., 1.75.0rc1), but pre-releases weren't enabled (try:
      `--prerelease=allow`)

      hint: Wheels are required for `grpcio-tools` because building from source is disabled for all packages (i.e., with `--no-build`)

      hint: Wheels are required for `nats-py` because building from source is disabled for all packages (i.e., with `--no-build`)

      hint: While the active Python version is 3.13, the resolution failed for other Python versions supported by your project. Consider limiting your
      project's supported Python versions using `requires-python`.
```

---

_Label `error messages` added by @konstin on 2025-09-19 22:37_

---

_Comment by @notatallshaw on 2025-09-21 03:46_

As an aside, also doesn't produce a helpful error in pip: https://github.com/pypa/pip/pull/13588

---
