```yaml
number: 4944
title: "Add support for `trusted-host` pip option"
type: pull_request
state: closed
author: fkapsahili
labels: []
assignees: []
draft: true
base: main
head: trusted-host-support
created_at: 2024-07-09T20:41:48Z
updated_at: 2024-08-27T17:19:36Z
url: https://github.com/astral-sh/uv/pull/4944
synced_at: 2026-01-10T13:09:50Z
```

# Add support for `trusted-host` pip option

---

_Pull request opened by @fkapsahili on 2024-07-09 20:41_

## Summary

The goal of this PR is to enable the `trusted-host` option like pip and to make it possible to bypass SSL validation when resolving and installing packages. With the `trusted-host` option it should be possible to specify a list of hosts that are ignored during validation. This option can be especially helpful when installing packages from private registries and behind corporate proxies.

@zanieb Can you take a look at the draft to see if I'm on the right track, especially with regard to the request handling of "secure" and "insecure" requests? Or do you see a possibility here with less refactoring involved? Currently I see no other option than setting `danger_accept_invalid_certs` to `true` when instantiating the `BaseClient`, but this would lead to two clients being maintained, as in my draft, since the request URL obviously cannot be determined at client build time.

Please note that this PR is still at an early stage and I would like to get early feedback on the possible implementation as I have very limited experience with Rust, but would love to help add this option to `uv`! 🙂

References: https://github.com/astral-sh/uv/issues/1339

## Test Plan
- [ ] We should probably add tests to test the behavior where we normally get an SSL verification error with a self-signed certificate
- [ ] Add tests to make sure that the `trusted-host` option is correctly applied to all existing configuration options

---

_Assigned to @zanieb by @zanieb on 2024-07-09 20:46_

---

_Comment by @samypr100 on 2024-07-10 01:11_

Curious, would the option mentioned in https://github.com/astral-sh/uv/issues/1339#issuecomment-2045458189 be more feasible and help avoid having to maintain two clients or would a small change be needed upstream in reqwest to support passing such configuration? I haven't looked into it too much but found the references of where its being used in reqwest. Ideally all it takes is one configuration to pass a NoVerifier-style impl to a selected set of hosts, that way only one client config would be needed.

* https://github.com/rustls/rustls/blob/main/rustls/src/webpki/server_verifier.rs#L35
* https://github.com/seanmonstar/reqwest/blob/master/src/tls.rs

---

_Comment by @timvink on 2024-07-11 07:10_

Looking forward to this one! We can promote the new feature in the README also: 

https://github.com/astral-sh/uv/blob/42cb2541b513a9d53d9c677610fd478e27162a74/README.md?plain=1#L111-L113

---

_Comment by @zanieb on 2024-07-11 14:36_

Thanks you for contributing! No worries that you don't have Rust experience we're happy to help out.

Did you explore the comment @samypr100 noted? I think we're all on the same page that it'd be ideal not to maintain multiple clients.

---

_Comment by @fkapsahili on 2024-07-12 07:14_

> Curious, would the option mentioned in [#1339 (comment)](https://github.com/astral-sh/uv/issues/1339#issuecomment-2045458189) be more feasible and help avoid having to maintain two clients or would a small change be needed upstream in reqwest to support passing such configuration? I haven't looked into it too much but found the references of where its being used in reqwest. Ideally all it takes is one configuration to pass a NoVerifier-style impl to a selected set of hosts, that way only one client config would be needed.
> 
> * https://github.com/rustls/rustls/blob/main/rustls/src/webpki/server_verifier.rs#L35
> * https://github.com/seanmonstar/reqwest/blob/master/src/tls.rs

Thanks for pointing this out!
Since the NoVerifier struct is responsible for disabling the certificate checks, I think we would need to add a method in the `ClientBuilder` to configure a `NoVerifier` for specific hosts, store these verifiers in a `HashMap`, and modify the connection logic to use these custom verifiers when making requests to the specified hosts. This would allow us to skip certificate checks for specific hosts and would allow us to continue to maintain only one client in uv:
https://github.com/seanmonstar/reqwest/blob/master/src/async_impl/client.rs

If this makes sense, I'll try to setup a PR in the `reqwest` library.

---

_Comment by @zanieb on 2024-07-19 17:08_

Do you mind if I close this so it's not in my review stack?

---

_Comment by @fkapsahili on 2024-07-21 18:11_

I am closing this as I am currently unable to continue working on this PR or the upstream changes in the `reqwest` library, although I would love to support the UV community with this! 😕

I also found that setting the `SSL_CERT_FILE` environment variable to the path of the corresponding root certificate solved my problem with an internal registry.

I hope this helps someone who has similar problems!

---

_Closed by @fkapsahili on 2024-07-21 18:11_

---

_Comment by @zanieb on 2024-08-27 17:19_

Hey @fkapsahili just wanted to say thanks — we ended up pursuing a multi-client approach in https://github.com/astral-sh/uv/pull/6591 :)

---
