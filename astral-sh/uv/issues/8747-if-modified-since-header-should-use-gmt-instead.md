```yaml
number: 8747
title: "`if-modified-since` header should use `GMT` instead of `-0000`"
type: issue
state: closed
author: camper42
labels:
  - bug
assignees: []
created_at: 2024-11-01T08:40:51Z
updated_at: 2024-11-01T13:55:47Z
url: https://github.com/astral-sh/uv/issues/8747
synced_at: 2026-01-12T15:59:34Z
```

# `if-modified-since` header should use `GMT` instead of `-0000`

---

_@camper42_

- command: `uv lock -U` uses our internal registry, backed by Tencent COS.
- uv platform: macOS
- uv version: uv 0.4.29 (Homebrew, 2024-10-30)

error log:
```
error: Failed to download `something==0.14.0`
  Caused by: Failed to fetch: `https://pypi.internal/api/package/something/something-0.14.0-py3-none-any.whl#sha256=ab3bae4f61c0ca8a72da6836610fd1256bd203420d7ec95be43afcc73512bcff`
  Caused by: HTTP status client error (400 Bad Request) for url (https://cos.tencent/our-bucket/packages/3751/something/something-0.14.0-py3-none-any.whl?Some_sig_params)
```

The issue is that COS reported: **The `If-Modified-Since` you specified is not valid.**

I checked the request header sent by UV, and it appears as follows: `if-modified-since: Wed, 17 Jul 2024 03:39:18 -0000`.

According to the [MDN documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Modified-Since), **HTTP dates should always be expressed in `GMT`, not in local time**.

Therefore, uv should adjust the header value to something like: `if-modified-since: Wed, 17 Jul 2024 03:39:18 GMT`.



---

_Comment by @camper42 on 2024-11-01 08:57_

RFC 2616 Hypertext Transfer Protocol -- HTTP/1.1:
- https://datatracker.ietf.org/doc/html/rfc2616#section-14.25
- https://datatracker.ietf.org/doc/html/rfc2616#section-3.3.1

---

_Comment by @camper42 on 2024-11-01 09:02_

https://datatracker.ietf.org/doc/html/rfc9110#section-13.1.3
https://datatracker.ietf.org/doc/html/rfc9110#section-5.6.7

---

_Assigned to @BurntSushi by @BurntSushi on 2024-11-01 11:56_

---

_Label `bug` added by @BurntSushi on 2024-11-01 11:56_

---

_Comment by @BurntSushi on 2024-11-01 12:22_

I responded [here](https://github.com/BurntSushi/jiff/issues/151) with a bit more on the datetime side of things.

[This PR](https://github.com/BurntSushi/jiff/pull/154) adds a new API to Jiff to make it easy to print RFC 9110 compatible timestamps. So once that's released, we just need to update uv to use that new API and I believe this should be fixed.

> According to the [MDN documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Modified-Since), HTTP dates should always be expressed in GMT, not in local time.

One clarification here is that the existing format is not in local time. It is semantically equivalent to GMT. It's using `-0000` to express that instead of `GMT`. But they are the same thing. Interestingly, RFC 2822/5322 considers `GMT` to be obsolete. But RFC 9110 requires it specifically.

(The other difference is that RFC 9110 requires a two digit day. This makes the value fixed length.)

---

_Closed by @BurntSushi on 2024-11-01 13:46_

---

_Closed by @BurntSushi on 2024-11-01 13:46_

---

_Comment by @camper42 on 2024-11-01 13:55_

> One clarification here is that the existing format is not in local time.

I just copied the whole sentence, sorry for the ambiguity, I checked the time and the package was modified at the right time (`Jul 17 11:39:18 2024 +0800`).

Thanks for the quick fix, it's one of the reasons why I love uv!

---
