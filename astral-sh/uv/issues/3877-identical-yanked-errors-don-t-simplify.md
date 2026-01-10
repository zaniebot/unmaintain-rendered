---
number: 3877
title: "Identical yanked errors don't simplify"
type: issue
state: closed
author: konstin
labels:
  - error messages
assignees: []
created_at: 2024-05-28T12:38:33Z
updated_at: 2024-06-27T11:25:27Z
url: https://github.com/astral-sh/uv/issues/3877
synced_at: 2026-01-10T01:23:31Z
---

# Identical yanked errors don't simplify

---

_Issue opened by @konstin on 2024-05-28 12:38_

```toml
dependencies = [
    "sentry >=2.0.1,<3",
]
```

```
 × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of sentry are available:
          sentry<=2.0.1
          sentry==2.0.2
          sentry==2.1.0
          sentry==2.1.1
          sentry==2.1.2
          sentry==2.1.3
          sentry==2.2.0
          sentry==2.2.1
          sentry==2.2.2
          sentry==2.2.3
          sentry==2.2.4
          sentry==2.2.5
          sentry==2.3.0
          sentry==2.3.1
          sentry==2.3.2
          sentry==2.4.0
          sentry==2.4.1
          sentry==2.4.2
          sentry==2.4.3
          sentry==2.4.4
          sentry==2.4.5
          sentry==2.4.6
          sentry==2.4.7
          sentry==2.5.0
          sentry==2.5.1
          sentry==2.5.2
          sentry==2.6.0
          sentry==2.6.1
          sentry==2.6.2
          sentry==2.7.0
          sentry==2.8.0
          sentry==2.8.1
          sentry==2.8.2
          sentry==2.9.0
          sentry>=3
      and sentry==2.0.1 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.0.2 cannot be used.
      And because sentry==2.0.2 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.1.0 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.1.1 cannot be used.
      And because sentry==2.1.1 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.1.2 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.1.3 cannot be used.
      And because sentry==2.1.3 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.2.0 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.2.1 cannot be used.
      And because sentry==2.2.1 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.2.2 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.2.3 cannot be used.
      And because sentry==2.2.3 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.2.4 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.2.5 cannot be used.
      And because sentry==2.2.5 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.3.0 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.3.1 cannot be used.
      And because sentry==2.3.1 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.3.2 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.4.0 cannot be used.
      And because sentry==2.4.0 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.4.1 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.4.2 cannot be used.
      And because sentry==2.4.2 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.4.3 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.4.4 cannot be used.
      And because sentry==2.4.4 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.4.5 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.4.6 cannot be used.
      And because sentry==2.4.6 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.4.7 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.5.0 cannot be used.
      And because sentry==2.5.0 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.5.1 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.5.2 cannot be used.
      And because sentry==2.5.2 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.6.0 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.6.1 cannot be used.
      And because sentry==2.6.1 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.6.2 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.7.0 cannot be used.
      And because sentry==2.7.0 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.8.0 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.8.1 cannot be used.
      And because sentry==2.8.1 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and sentry==2.8.2 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654), we can
      conclude that sentry>=2.0.1,<2.9.0 cannot be used.
      And because sentry==2.9.0 was yanked (reason: https://github.com/getsentry/self-hosted/issues/1654)
      and github-wikidata-bot depends on sentry>=2.0.1,<3, we can conclude that the requirements are
      unsatisfiable.
```

---

_Label `error messages` added by @konstin on 2024-05-28 12:38_

---

_Comment by @konstin on 2024-06-27 11:25_

Closing as a special case of #1901

---

_Closed by @konstin on 2024-06-27 11:25_

---
