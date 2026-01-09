---
number: 8909
title: "Feature req: some way to estimate dependency size"
type: issue
state: open
author: dimaqq
labels:
  - wish
  - cache
assignees: []
created_at: 2024-11-08T00:36:49Z
updated_at: 2024-11-09T11:16:33Z
url: https://github.com/astral-sh/uv/issues/8909
synced_at: 2026-01-07T13:12:18-06:00
---

# Feature req: some way to estimate dependency size

---

_Issue opened by @dimaqq on 2024-11-08 00:36_

uv 0.4.30 (Homebrew 2024-11-05)

```command
🦐/c/test-otel (main)> uv tree
Resolved 22 packages in 14ms
test-otel v0.1.0
└── opentelemetry-exporter-otlp v1.28.0
    ├── opentelemetry-exporter-otlp-proto-grpc v1.28.0
    │   ├── deprecated v1.2.14
    │   │   └── wrapt v1.16.0
    │   ├── googleapis-common-protos v1.65.0
    │   │   └── protobuf v5.28.3
    │   ├── grpcio v1.67.1
    │   ├── opentelemetry-api v1.28.0
    │   │   ├── deprecated v1.2.14 (*)
    │   │   └── importlib-metadata v8.5.0
    │   │       └── zipp v3.20.2
    │   ├── opentelemetry-exporter-otlp-proto-common v1.28.0
    │   │   └── opentelemetry-proto v1.28.0
    │   │       └── protobuf v5.28.3
    │   ├── opentelemetry-proto v1.28.0 (*)
    │   └── opentelemetry-sdk v1.28.0
    │       ├── opentelemetry-api v1.28.0 (*)
    │       ├── opentelemetry-semantic-conventions v0.49b0
    │       │   ├── deprecated v1.2.14 (*)
    │       │   └── opentelemetry-api v1.28.0 (*)
    │       └── typing-extensions v4.12.2
    └── opentelemetry-exporter-otlp-proto-http v1.28.0
        ├── deprecated v1.2.14 (*)
        ├── googleapis-common-protos v1.65.0 (*)
        ├── opentelemetry-api v1.28.0 (*)
        ├── opentelemetry-exporter-otlp-proto-common v1.28.0 (*)
        ├── opentelemetry-proto v1.28.0 (*)
        ├── opentelemetry-sdk v1.28.0 (*)
        └── requests v2.32.3
            ├── certifi v2024.8.30
            ├── charset-normalizer v3.4.0
            ├── idna v3.10
            └── urllib3 v2.2.3
(*) Package tree already displayed
```

Now, when I was adding the packages, I did see that `grpcio` was a whopping 10MB.

While it's relatively easy to check the total venv size on disk, it would be amazing to be able to highlight the contribution of each individual package or a subtree of packages or something.

I can poke inside `uv.lock`, but there are many wheels listed for each package, it's unclear which would be used.

I guess a complete feature could hypothetically look like "simulate installation on py3.13/linux/arm64 and print each package wheel size and size on disk"...

But frankly, something simpler would be more than enough.

For example `uv pip freeze --sizes` or `--wheel-sizes1` to report my current venv usage by pacakge.

---

_Label `wish` added by @charliermarsh on 2024-11-09 02:17_

---

_Label `cache` added by @charliermarsh on 2024-11-09 02:17_

---

_Comment by @charliermarsh on 2024-11-09 02:19_

We could pretty easily include the wheel size here? Including the _unzipped_ size on disk or in the installed environment would be harder but I doubt the difference between these various things really matters. Would that be useful?

---

_Comment by @dimaqq on 2024-11-09 11:16_

A mere hint at dep size would be miles better than nothing :)

Wheel size is quite reasonable, I think.

I was thinking that given how uv hardlinks or copies stuff it would be aware of sizes of those things, but maybe that assumption was wrong.

---
