```yaml
number: 14199
title: Add proto and tlsv1.2 to curl commands
type: pull_request
state: closed
author: pstoeckle
labels:
  - enhancement
  - needs-decision
assignees: []
base: main
head: main
created_at: 2025-06-22T11:21:55Z
updated_at: 2025-06-27T17:59:08Z
url: https://github.com/astral-sh/uv/pull/14199
synced_at: 2026-01-12T16:11:04Z
```

# Add proto and tlsv1.2 to curl commands

---

_@pstoeckle_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Use `--proto '=https' --tlsv1.2` in `curl` commands.

## Test Plan

Added the flags also to `.github/workflows/ci.yml`. Thus, if the CI works, it should also be working in general.


---

_Renamed from "Add proto and tlsv1.2 to curl commands in installer script" to "Add proto and tlsv1.2 to curl commands" by @pstoeckle on 2025-06-22 11:26_

---

_Review requested from @Gankra by @konstin on 2025-06-22 11:31_

---

_Label `enhancement` added by @konstin on 2025-06-22 11:31_

---

_Comment by @zanieb on 2025-06-22 12:56_

Hi! Thanks for the pull request.

Can you share your motivation?

---

_Comment by @pstoeckle on 2025-06-22 13:30_

With `--tlsv1.2`, `curl` only allows modern/secure TLS suites. 

With `--proto '=https'`, `curl` only allows `https`, and no `http`.

You probably already configure your servers similarly, but with these options you can make it explicit on the client side.

---

_Comment by @Gankra on 2025-06-23 13:59_

So the motivation for these changes is to define a minimum acceptable protocol to basically guard against downgrade attacks, where a server claims it can only support <ancient obsolete protocol> or a redirect strips https. AFAICT the value of this is marginal but it's a nice little ritual to make things that little bit more secure.

These are exactly the flags that [rustup includes](https://rustup.rs/), and [that cargo-dist tells you to use](https://github.com/astral-sh/uv/releases/). [Folks have argued for rustup to bump the minimum to tlsv1.3 but the longtail spectre of weird old machines was deemed worth supporting](https://github.com/rust-lang/rustup/issues/2581).

I seem to recall astral stripped these for some obscure compatibility reason but I can't recall the details and no longer have access to the relevant chatlogs.

---

_Comment by @Gankra on 2025-06-23 14:02_

I would bias to accepting this unless someone (@charliermarsh?) can remember why those flags had to be dropped.

---

_Comment by @konstin on 2025-06-26 19:28_

I tried a debian bulleye, the oldest non-EOL debian, and it rejects everything older than TLS v1.2:

```
$ curl -LsSf --tls-max 1.1 https://astral.sh/uv/install.sh
curl: (35) error:141E70BF:SSL routines:tls_construct_client_hello:no protocols available
```

A CentOS 7 server I have access to acts similarly:

```
$ curl -LsSf --tls-max 1.1 https://astral.sh/uv/install.sh
curl: (35) OpenSSL/1.0.2k-fips: error:1407742E:SSL routines:SSL23_GET_SERVER_HELLO:tlsv1 alert protocol version
```

openSUSE Leap 15.3:

```
$ curl -LsSf --tls-max 1.1 https://astral.sh/uv/install.sh
curl: (35) error:1409442E:SSL routines:ssl3_read_bytes:tlsv1 alert protocol version
```

Only on Ubuntu 18.04 with extended security maintenance support TLS v1.1.

From that perspective, we're already guaranteeing TLS v1.2 on any reasonably up-to-date system.

---

_Review request for @Gankra removed by @konstin on 2025-06-26 19:28_

---

_Label `needs-decision` added by @konstin on 2025-06-26 19:29_

---

_Comment by @zanieb on 2025-06-27 17:59_

I think we're leaning no on this. Thanks for taking the time to contribute though!

---

_Closed by @zanieb on 2025-06-27 17:59_

---
