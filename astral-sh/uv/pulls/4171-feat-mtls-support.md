```yaml
number: 4171
title: "feat: mTLS support"
type: pull_request
state: merged
author: samypr100
labels:
  - enhancement
assignees: []
merged: true
base: main
head: mutual-tls
created_at: 2024-06-09T16:14:06Z
updated_at: 2024-06-11T04:56:31Z
url: https://github.com/astral-sh/uv/pull/4171
synced_at: 2026-01-12T16:06:04Z
```

# feat: mTLS support

---

_@samypr100_

## Summary

Closes https://github.com/astral-sh/uv/issues/3626

This adds mTLS support to uv via the standard env var `SSL_CLIENT_CERT`.

## Test Plan

Tested locally using a [nginx proxy to pypi](https://github.com/hauntsaninja/nginx_pypi_cache) using my own self-signed ca + certs + client certs generated via [mkcert](https://github.com/FiloSottile/mkcert). Used this proxy with both uv and pip to make sure we have feature partity in mTLS functionality.


---

_Assigned to @zanieb by @zanieb on 2024-06-09 16:16_

---

_Review requested from @zanieb by @zanieb on 2024-06-09 16:16_

---

_Marked ready for review by @samypr100 on 2024-06-09 16:21_

---

_Comment by @samypr100 on 2024-06-09 16:34_

I didn't see `--cert` supported as a direct CLI option given we delegate `SSL_CERT_FILE` to upstream rust tls. Similarly this PR focuses on only supporting the client cert environment var.

I think we could explore support via config file in a future PR if it makes sense for uv to do so, thoughts?


---

_Comment by @zanieb on 2024-06-09 16:56_

Thanks for putting this up!

> I think we could explore support via config file in a future PR if it makes sense for uv to do so, thoughts?

Totally fine with me to separate the implementation

---

_@zanieb reviewed on 2024-06-10 14:20_

---

_Review comment by @zanieb on `README.md`:489 on 2024-06-10 14:20_

I'd omit a preface like that, who knows what else we'll add in the future :)

```suggestion
If client certificate authentication (mTLS) is desired, set the `SSL_CLIENT_CERT`
```


---

_Review comment by @zanieb on `crates/uv-client/src/middleware.rs`:59 on 2024-06-10 19:08_

This feel like kind of a weird place for this to live. Any particular reason you put it in the middleware module?

---

_@zanieb reviewed on 2024-06-10 19:08_

---

_@zanieb approved on 2024-06-10 19:08_

---

_Label `enhancement` added by @zanieb on 2024-06-10 19:08_

---

_@samypr100 reviewed on 2024-06-10 23:18_

---

_Review comment by @samypr100 on `crates/uv-client/src/middleware.rs`:59 on 2024-06-10 23:18_

I didn't think about it too much. Just thought its specific for reqwest usage so I saw it as middleware. Should I move these to a new `tls.rs` file?

---

_@samypr100 reviewed on 2024-06-10 23:21_

---

_Review comment by @samypr100 on `crates/uv-client/src/middleware.rs`:59 on 2024-06-10 23:21_

Moved in d9f04b83843b5154099d4a53b5435df1fe873962

---

_@zanieb reviewed on 2024-06-11 01:11_

---

_Review comment by @zanieb on `crates/uv-client/src/middleware.rs`:59 on 2024-06-11 01:11_

Ah I would have just put it in the `base_client` module since it's just used for configuration. This works though, thanks!

---

_@zanieb approved on 2024-06-11 01:11_

---

_Merged by @zanieb on 2024-06-11 01:11_

---

_Closed by @zanieb on 2024-06-11 01:11_

---

_Branch deleted on 2024-06-11 04:56_

---
