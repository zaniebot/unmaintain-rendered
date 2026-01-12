```yaml
number: 6591
title: "Add support for `--trusted-host`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
  - configuration
  - security
assignees: []
merged: true
base: main
head: charlie/trust
created_at: 2024-08-24T21:51:11Z
updated_at: 2024-08-27T19:21:05Z
url: https://github.com/astral-sh/uv/pull/6591
synced_at: 2026-01-12T16:07:26Z
```

# Add support for `--trusted-host`

---

_@charliermarsh_

## Summary

This PR revives https://github.com/astral-sh/uv/pull/4944, which I think was a good start towards adding `--trusted-host`. Last night, I tried to add `--trusted-host` with a custom verifier, but we had to vendor a lot of `reqwest` code and I eventually hit some private APIs. I'm not confident that I can implement it correctly with that mechanism, and since this is security, correctness is the priority.

So, instead, we now use two clients and multiplex between them.

Closes https://github.com/astral-sh/uv/issues/1339.

## Test Plan

Created self-signed certificate, and ran `python3 -m http.server --bind 127.0.0.1 4443 --directory . --certfile cert.pem --keyfile key.pem` from the packse index directory.

Verified that `cargo run pip install transitive-yanked-and-unyanked-dependency-a-0abad3b6 --index-url https://127.0.0.1:8443/simple-html` failed with:

```
error: Request failed after 3 retries
  Caused by: error sending request for url (https://127.0.0.1:8443/simple-html/transitive-yanked-and-unyanked-dependency-a-0abad3b6/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: Other(OtherError(CaUsedAsEndEntity))
```

Verified that `cargo run pip install transitive-yanked-and-unyanked-dependency-a-0abad3b6 --index-url 'https://127.0.0.1:8443/simple-html' --trusted-host '127.0.0.1:8443'` failed with the expected error (invalid resolution) and made valid requests.

Verified that `cargo run pip install transitive-yanked-and-unyanked-dependency-a-0abad3b6 --index-url 'https://127.0.0.1:8443/simple-html' --trusted-host '127.0.0.2' -n` also failed.


---

_Label `compatibility` added by @charliermarsh on 2024-08-24 21:51_

---

_Label `configuration` added by @charliermarsh on 2024-08-24 21:51_

---

_Label `security` added by @charliermarsh on 2024-08-24 21:51_

---

_@charliermarsh reviewed on 2024-08-24 22:45_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/settings.rs`:219 on 2024-08-24 22:45_

Ahh URL is wrong -- it's just a host or host:port pair.

---

_Review requested from @zanieb by @charliermarsh on 2024-08-24 23:15_

---

_@charliermarsh reviewed on 2024-08-24 23:16_

---

_Review comment by @charliermarsh on `crates/uv-client/src/base_client.rs`:292 on 2024-08-24 23:16_

The only way to access this client is via `for_host`. It's not exposed anywhere else.

---

_@samypr100 reviewed on 2024-08-24 23:22_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-24 23:22_

Would it be reasonable to also warn the user once on the usage of this option / mitm implications? (e.g. mostly making sense on trusted private registries/mirrors or self-signed corporate pypi proxies)?

---

_@blu3r4d0n reviewed on 2024-08-24 23:43_

---

_Review comment by @blu3r4d0n on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-24 23:43_

I think that makes sense. Although since it's opt-in maybe they would already be aware?

---

_@charliermarsh reviewed on 2024-08-24 23:48_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-24 23:48_

As in, show a warning on the CLI?

---

_@blu3r4d0n reviewed on 2024-08-24 23:59_

---

_Review comment by @blu3r4d0n on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-24 23:59_

I think that's what Sammy means. Iirc, pip doesn't show a warning.

---

_@samypr100 reviewed on 2024-08-25 00:00_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 00:00_

Indeed, I was thinking CLI warning.

---

_@gaby reviewed on 2024-08-25 00:12_

---

_Review comment by @gaby on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 00:12_

`pip` doesnt show a warning neither should `uv`, it's an opt-in option. It's the same as doing `curl -k https://fakehost.com`, which doesnt have a warning.

uv should show an error if the user tries to install from a non-trusted source without using "--trusted-hosts"

---

_@samypr100 reviewed on 2024-08-25 00:12_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 00:12_

Here's an initial stab at it

```
Only use `--trusted-host` in a secure network with verified sources, as it bypasses SSL verification and could expose you to MITM attacks.
```

---

_@blu3r4d0n reviewed on 2024-08-25 00:13_

---

_Review comment by @blu3r4d0n on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 00:13_

I think gaby is right here. Follow pip's approach. Could add a line in docs indicating pitfalls.

---

_@samypr100 reviewed on 2024-08-25 00:20_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 00:20_

You often have cases like this https://github.com/astral-sh/uv/issues/1339#issuecomment-2232619423 where using the appropriate `UV_NATIVE_TLS` or `SSL_CERT_FILE` could've sufficed instead of a more dangerous option like `--trusted-host`.  This SO's warning also speaks for itself https://stackoverflow.com/a/29751768 as to the dangers of blindly using this option and the effect it can have if used inappropriately.

---

_@inoa-jboliveira reviewed on 2024-08-25 00:23_

---

_Review comment by @inoa-jboliveira on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 00:23_

(lurker here) I would prefer the documentation for this command line option to have the warning but not on the actual usage. Rationale: the command is called "trusted-host" as in I trust this thing whatever it may be. This option is also likely to live in the uv.toml or pyproject.toml or env variables. Meaning every single call to uv will have a warning

---

_Comment by @zanieb on 2024-08-25 00:24_

Also might be helpful for #5726 

---

_Comment by @gaby on 2024-08-25 00:30_

What happens if `--trusted-host` is supplied multiple times, is that taken into account? Or do the later override the first one?

Edit: I see the code allows a list, which if it's like `pip` it would be space-separated.

---

_@gaby reviewed on 2024-08-25 00:40_

---

_Review comment by @gaby on `crates/uv-cli/src/lib.rs`:1588 on 2024-08-25 00:40_

Where is this `space` delimiter tested? I couldn't find it in the PR changes

---

_@samypr100 reviewed on 2024-08-25 00:43_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 00:43_

> I trust this thing whatever it may be

Trust is vaguely defined in the name of the option, for example `--trusted-host pypi.org --trusted-host pypi.python.org` hosts may be trusted since they're official, but in an insecure network location it can expose your system to the dangers aformentioned.

---

_@samypr100 reviewed on 2024-08-25 13:07_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 13:07_

@charliermarsh

Given the above feedback if we want to have a self-documenting flag and skip the warning, I propose renaming the option to more aligned with other tools e.g. curl's `--insecure`, k8s `--insecure-skip-tls-verify`, docker's `--insecure-registry` etc .

Hence instead of `--trusted-host`, using something like `--insecure-host` or `--insecure-skip-verify-host`?

uv has a chance to improve this area rather than to stick with the same name which has been easily misunderstood on the security implications over the years. This also can be documented in pip compatibility guide like the other cases.

---

_Review comment by @gaby on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 13:08_

@samypr100 `trusted-host` is the name used by `pip`.

It implies i'm trusting that host, it's a choice.

---

_@gaby reviewed on 2024-08-25 13:08_

---

_@samypr100 reviewed on 2024-08-25 13:18_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 13:18_

@gaby Yes, I proposed this knowing its the original name used by `pip`. `uv` doesn't have to keep the same name.

---

_@gaby reviewed on 2024-08-25 13:21_

---

_Review comment by @gaby on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 13:21_

Yeah, `--insecure-host` should work too. Simple and clear.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 14:10_

I think we could say something like `--unsafe-insecure-host`?  I think regardless, we probably need to provide an alias to `--trusted-host`. Though we really feel strongly about a new name we could use our `CompatArgs` utility to suggest using the new flag instead.

---

_@zanieb reviewed on 2024-08-25 14:10_

---

_@zanieb reviewed on 2024-08-25 14:14_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2015 on 2024-08-25 14:14_

I don't think we say "A list" for most options that take multiple values. I'd say like...


> Allow insecure connections to a host.
>
> Can be provided multiple times.
> 
> ...

We also should be a bit more verbose about the warning and the implications.

---

_@charliermarsh reviewed on 2024-08-25 14:22_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 14:22_

`--allow-insecure-host`?

---

_Review comment by @zanieb on `crates/uv-configuration/src/trusted_host.rs`:117 on 2024-08-25 14:22_

Can you add a test case for something with a path? e.g. `https://example.com/hello/world`?

---

_@zanieb reviewed on 2024-08-25 14:22_

---

_@zanieb reviewed on 2024-08-25 14:24_

---

_Review comment by @zanieb on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:24_

I think you can / should use `Realm` in place of this (`crates/uv-auth/src/realm.rs`) — I think you can just move the `from_str` implementation over. I'm not sure if that adds complexity to the setting schema part — but it seems like we could just have `Realm` in the setting schema.

---

_@zanieb reviewed on 2024-08-25 14:25_

---

_Review comment by @zanieb on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:25_

"should" because it's our canonical way to do this and there's `From<&Url> for Realm`.

---

_@charliermarsh reviewed on 2024-08-25 14:25_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:25_

Doesn't that require a scheme? These values don't require a scheme.

---

_@zanieb reviewed on 2024-08-25 14:28_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 14:28_

Just felt like we should reuse our `unsafe` concept from index strategies, but it does get pretty verbose.. `--allow-insecure-host` is okay.

---

_@zanieb reviewed on 2024-08-25 14:28_

---

_Review comment by @zanieb on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:28_

It does yeah, hm. But if someone provides a scheme here (which is allowed per the parser), shouldn't that match be enforced?

---

_@zanieb reviewed on 2024-08-25 14:30_

---

_Review comment by @zanieb on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:30_

The scheme could become optional on `Realm`, I'm not sure if that causes problems. I don't feel strongly if it seems like a pain. There's not a lot of complexity in the matching anymore (`Realm` got simplified at some point, relying on some behaviors from `reqwest`)

---

_@charliermarsh reviewed on 2024-08-25 14:31_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:31_

Yeah I agree. Should we assume HTTPS if omitted...? It would be strange to put that in `FromStr`, but it could make sense for _this_ setting. Or could scheme be optional on `Realm`?

---

_@zanieb reviewed on 2024-08-25 14:32_

---

_Review comment by @zanieb on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:32_

I'm not sure, I feel like if you do `--trusted-host example.com` and we make a request to `http://example.com`— does skipping SSL verification even matter or make sense? It seems reasonable to assume `https` since it's only relevant there?

---

_@zanieb reviewed on 2024-08-25 14:33_

---

_Review comment by @zanieb on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:33_

And I guess another point this raises: should we allow `--trusted-host http://example.com`? What does that mean?

---

_@charliermarsh reviewed on 2024-08-25 14:34_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:34_

I suppose we could reject `http`.

---

_@gaby reviewed on 2024-08-25 14:39_

---

_Review comment by @gaby on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:39_

We should allow `http://`. It's normal practice to run internal pypi mirror that runs on http. That's the whole point of `trusting` a host.

---

_@gaby reviewed on 2024-08-25 14:40_

---

_Review comment by @gaby on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:40_

Same applies to, lets say the mirror is https but uses a custom/internal/enterprise CA.

You would want to install from it without verifying the cert, for example inside a Docker build, where you dont want to be adding custom CA's in the container.

---

_@charliermarsh reviewed on 2024-08-25 14:43_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:43_

If you're running over `http://`, why would certificate validity matter?

---

_@gaby reviewed on 2024-08-25 14:44_

---

_Review comment by @gaby on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:44_

The following use-cases should work with --trusted-host/--insecure-host or whichever is the final name.
- http://example.com
- https://example.com
- https://example.com (Don't verify CA)

---

_@charliermarsh reviewed on 2024-08-25 14:45_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:45_

Sorry, I'm not following. Why is `--trusted-host` necessary _at all_ for an `http://` domain?

---

_@charliermarsh reviewed on 2024-08-25 14:47_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:47_

I guess pip warns if you try to use `http://`? We don't do that. I don't think you need `--trusted-host` at all for `http://` domains with uv. This is only relevant to `https://`.

---

_@gaby reviewed on 2024-08-25 14:48_

---

_Review comment by @gaby on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:48_

You are right, there's no verification needed.

---

_@zanieb reviewed on 2024-08-25 14:48_

---

_Review comment by @zanieb on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:48_

Yeah I feel like setting an index URL to `http://example.com` works today (e.g., I'm pretty sure it was fine in #5726) but setting `ALL_PROXY` didn't work.

---

_@zanieb reviewed on 2024-08-25 14:49_

---

_Review comment by @zanieb on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:49_

We might need to test what happens if you set `HTTPS_PROXY` or `ALL_PROXY` to an `http://` URL?

---

_@gaby reviewed on 2024-08-25 14:52_

---

_Review comment by @gaby on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:52_

@zanieb That's typically what I use when using something like Squid-Proxy + Pypi. I set both `HTTP_PROXY` and `HTTPS_PROXY` to "http://squid-hostname`, and let Squid handle the request/redirect/etc.

---

_@charliermarsh reviewed on 2024-08-25 14:54_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 14:54_

Will do.

---

_@samypr100 reviewed on 2024-08-25 14:59_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 14:59_

I like `--allow-insecure-host` 💯 

> I think regardless, we probably need to provide an alias to --trusted-host.

I think this is where I'd go back to my original suggestion of including a warning when `--trusted-host` is used (if such alias exists) given the confusion around what it means and the security implications as it shies away from it's true meaning and associated dangers mentioned in https://github.com/astral-sh/uv/pull/6591#discussion_r1730176753.

---

_@samypr100 reviewed on 2024-08-25 15:23_

---

_Review comment by @samypr100 on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 15:23_

> We should allow `http://`. It's normal practice to run internal pypi mirror that runs on http. That's the whole point of `trusting` a host.

This is a reason as to why I'm suggesting a rename away from `--trusted-host`  as it's not about trusting a host, rather skipping TLS verification 😅 

---

_@charliermarsh reviewed on 2024-08-25 15:45_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/trusted_host.rs`:10 on 2024-08-25 15:45_

@zanieb - I opted for now to keep `TrustedHost`, because the requirements are different than `Realm`... `Realm` requires a scheme, `TrustedHost` requires a host (`Realm` does not), etc. I was also hesitant to make `uv-auth` a dependency everywhere, and add `Schemars` as a dep to that crate.

---

_@inoa-jboliveira reviewed on 2024-08-25 18:03_

---

_Review comment by @inoa-jboliveira on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 18:03_

I believe you might be missing the mark with the insecure thing. The connection is secure as in it will still use TLS (transport layer security). It is encrypted by a valid certificate of unknown origin. Note even if you say to trust it, the certificate still must be valid! 

You have all the benefits of the https connection. Saying this is insecure is comparing this to http which is indeed insecure.

Trusted is the correct word here.

Also this is not only for self signed certificates. There are older systems not recognizing newer CAs such as Let's encrypt (our case for needing this feature).



---

_@samypr100 reviewed on 2024-08-25 18:16_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 18:16_

@inoa-jboliveira It's insecure by definition to not have tls verification of the name. Anyone can claim they have a valid certificate otherwise and give you a false sense of security.

Note, using `insecure` was also suggested by @pradyunsg in the discussion of possible rename/removal of `--trusted-host` from pip in https://github.com/pypa/pip/issues/7725#issuecomment-899035686 started by @graingert

---

_@inoa-jboliveira reviewed on 2024-08-25 18:27_

---

_Review comment by @inoa-jboliveira on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 18:27_

@samypr100
By this definition, you must show same warning on any HTTP host being used. But it won't happen. 

Just FYI: Regardless of the name you pick by whatever reason you do, I thank the astral team for providing the feature I need the most for wide deploy of uv in my org. You guys rock

---

_@samypr100 reviewed on 2024-08-25 18:52_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:1581 on 2024-08-25 18:52_

>By this definition, you must show same warning on any HTTP host being used. But it won't happen.

This option only applies to https connections, not http.

---

_Comment by @pradyunsg on 2024-08-26 11:09_

> You’re receiving notifications because you were mentioned. 

Not sure why GitHub thinks I have a mention here... this seems fine to me, in case someone mentioned me for asking my opinion on the idea. :)


---

_Review requested from @zanieb by @charliermarsh on 2024-08-26 18:43_

---

_@zanieb approved on 2024-08-27 12:41_

LGTM. Might want to add a note around https://docs.astral.sh/uv/configuration/authentication/#custom-ca-certificates

---

_Comment by @graingert on 2024-08-27 12:45_

> > You’re receiving notifications because you were mentioned.
> 
> Not sure why GitHub thinks I have a mention here... this seems fine to me, in case someone mentioned me for asking my opinion on the idea. :)

You were tagged here https://github.com/astral-sh/uv/pull/6591#discussion_r1730406532

---

_Merged by @charliermarsh on 2024-08-27 13:36_

---

_Closed by @charliermarsh on 2024-08-27 13:36_

---

_Branch deleted on 2024-08-27 13:36_

---

_Comment by @jonathanasdf on 2024-08-27 19:10_

I seem to be getting some errors, eg.

```
[tool.uv]
allow-insecure-host = [
  "amazonaws.com",
]
```

```
warning: Failed to parse `pyproject.toml` during settings discovery:
  TOML parse error at line 93, column 1
     |
  93 | [tool.uv]
     | ^^^^^^^^^
  invalid type: string "amazonaws.com", expected struct TrustedHost
```

---

_Comment by @charliermarsh on 2024-08-27 19:11_

Apologies, that's my bad. Can you open a separate issue? It should work on the command-line though.

---

_Comment by @charliermarsh on 2024-08-27 19:21_

See: https://github.com/astral-sh/uv/pull/6716

---
