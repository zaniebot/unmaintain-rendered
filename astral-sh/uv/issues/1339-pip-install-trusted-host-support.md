```yaml
number: 1339
title: "`pip install --trusted-host` support"
type: issue
state: closed
author: stefanvanburen
labels:
  - enhancement
  - help wanted
  - network
assignees: []
created_at: 2024-02-15T21:00:04Z
updated_at: 2024-10-11T19:00:47Z
url: https://github.com/astral-sh/uv/issues/1339
synced_at: 2026-01-12T15:58:26Z
```

# `pip install --trusted-host` support

---

_@stefanvanburen_

`pip install` has the `trusted-host` flag:

```
--trusted-host <hostname>   Mark this host or host:port pair as trusted, even though it does not have valid or any HTTPS.
```

Seems like a nice-to-have for `uv pip install` to also support this flag.

---

_Comment by @zanieb on 2024-02-15 21:43_

Hi! Thanks for your feedback. Could you explain why this is valuable to you? Not saying we shouldn't have it, just want to learn more about use-cases.

---

_Label `enhancement` added by @zanieb on 2024-02-15 21:43_

---

_Comment by @stefanvanburen on 2024-02-15 21:51_

Of course! I work on developing a [PyPI-compatible repository](https://buf.build/docs/bsr/generated-sdks/python) that I'll occasionally run locally either without https or using self-signed certificates, in which case I need to supply the `--trusted-host` flag for the domain with our self-signed certs 😄. I'd also imagine that other users might occasionally need this for installing from internal PyPI mirrors, etc.

---

_Comment by @zanieb on 2024-02-15 22:01_

Sweet thanks! We ran into something like this in https://github.com/astral-sh/uv/pull/609 / https://github.com/astral-sh/uv/pull/615

---

_Comment by @atmartinezsf on 2024-02-15 23:21_

This is a need I have to use with an internal mirror/index. I would love to see this implemented. 

---

_Comment by @edwardpeek-crown-public on 2024-02-15 23:38_

This is perhaps tangential to this exact issue, but we'd like to see better support for _secure_ connections to registries with custom CAs too.

Right now we see `error trying to connect: invalid peer certificate: UnknownIssuer` errors connecting to a organisation pypi mirror with a custom CA installed to the system cert store. pip provides the ability to set `global.cert='/etc/ssl/certs/ca-certificates.crt'` for this use case.

---

_Comment by @zanieb on 2024-02-15 23:45_

Thanks @edwardpeek-crown ! I think we'll need to expose something like we explored in https://github.com/astral-sh/uv/pull/615

---

_Comment by @atmartinezsf on 2024-02-15 23:45_

The method @edwardpeek-crown pointed to is the way we usually implement our local config, but trusted host would work for us. I would be happy to see either implementation to allow the use of an internal mirror/registry. 

---

_Comment by @mickael-mounier on 2024-02-16 06:26_

Hello, I have a similar need here. We're using an internal devpi repo with a certificate signed by an internal root CA. Those are trusted by my workstation's Windows certificate store but I'm still getting an `invalid peer certificate: UnknownIssuer` error. Uv is currently unuseable for us without a way to trust a host or provide some kind of certificate store.

Thank you!

---

_Comment by @humanzz on 2024-02-16 21:03_

Coming from #1535 where I originally had a request for both `PIP_INDEX_URL` and `PIP_TRUSTED_HOST`.
Looks like setting the index via an environment variable is supported via `UV_INDEX_URL`.

So, related to this request for `--trusted-host`, it'd be great to also have it configurable via an environment variable - maybe `UV_TRUSTED_HOST` which in my case I want to leverage with an non-https urls for the index e.g. `UV_TRUSTED_HOST='127.0.0.1'`

---

_Label `network` added by @zanieb on 2024-02-28 18:28_

---

_Comment by @edwardpeek-crown-public on 2024-03-04 21:26_

Linking #1474 which solved a similar use case for us.

---

_Comment by @DesmondChoy on 2024-03-26 09:35_

+1 for uv to support `trusted-host` flag.

---

_Comment by @IbarraSamuel on 2024-03-29 00:56_

+1. Waiting for this feature so we can use uv as the default in my work team.

---

_Comment by @sovaa on 2024-03-29 04:19_

+1. Seems like a superb tool, but we can't use it in our team without trusted-host support.

---

_Comment by @zanieb on 2024-03-29 04:19_

Please don't comment with +1s, just upvote the original post. We'd like to keep the issue focused on substantive discussion and updates on implementation for all those subscribed.

The next step here is a prototype of how we would accomplish this, i.e. `reqwest` supports allowing invalid certificates (https://github.com/seanmonstar/reqwest/issues/182#issuecomment-469997565) but I'm not sure how we can do that per host or request.

---

_Comment by @zanieb on 2024-03-29 04:23_

I'd also like to see examples of tools other than `pip` that expose a flag to allow invalid certificates.

---

_Comment by @sovaa on 2024-03-29 06:59_

E.g. Docker has a similar feature called `--insecure-registry=http://...` when pulling images.

---

_Comment by @carlosjourdan on 2024-03-29 22:56_

--insecure-skip-tls-verify on kubectl

On Fri, Mar 29, 2024, 01:23 Zanie Blue ***@***.***> wrote:

> I'd also like to see examples of tools other than pip that expose a flag
> to allow invalid certificates.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/1339#issuecomment-2026624846>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AEF5Z5E444MOIYQMYFWB5PDY2TUDVAVCNFSM6AAAAABDK6NKISVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDAMRWGYZDIOBUGY>
> .
> You are receiving this because you are subscribed to this thread.Message
> ID: ***@***.***>
>


---

_Comment by @carlosjourdan on 2024-03-30 01:47_

Hashicorp vault apparently also supports this with the environment variable [VAULT_SKIP_VERIFY](https://developer.hashicorp.com/vault/docs/commands#vault_skip_verify)

---

_Comment by @jasonwmcswain on 2024-04-02 16:00_

Where I work, there is an internal Pypi mirror which is also used to uploading our internal pypi packages.  Unfortunately, IT has configured these hosts with "HTTP", so I have been providing both of the following args to our pip install commands. "--trusted-host" and "--extra-index-url".  

Please add support for both, so that I can onboard to "uv".  we are already using ruff, and it is blazing fast.   I am very excited to use uv as well.

---

_Comment by @carlosjourdan on 2024-04-02 16:48_

I believe that with http, if you remove the trusted-host and keep the
extra-index-url, things should work fine. For me, the problem only arises
on https with self signed certificates, which is common behind a corporate
firewall.

On Tue, Apr 2, 2024, 13:01 Jason ***@***.***> wrote:

> Where I work, there is an internal Pypi mirror which is also used to
> uploading our internal pypi packages. Unfortunately, IT has configured
> these hosts with "HTTP", so I have been providing both of the following
> args to our pip install commands. "--trusted-host" and "--extra-index-url".
>
> Please add support for both, so that I can onboard to "uv". we are already
> using ruff, and it is blazing fast. I am very excited to use uv as well.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/1339#issuecomment-2032451860>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AEF5Z5C6XONT44KWK7IN3SLY3LI45AVCNFSM6AAAAABDK6NKISVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDAMZSGQ2TCOBWGA>
> .
> You are receiving this because you are subscribed to this thread.Message
> ID: ***@***.***>
>


---

_Comment by @inoa-jboliveira on 2024-04-09 14:50_

>  but I'm not sure how we can do that per host or request.

You can check if the host is the same passed via `--trusted-host` and add the flag to reqwest. Also it is important to be explicit here instead of a catch-all command line argument to allow any certificate. It should be per host

---

_Comment by @zanieb on 2024-04-09 14:52_

@inoa-jboliveira is there an API to do so per request? We use a shared client for all of the requests we make.

---

_Comment by @inoa-jboliveira on 2024-04-09 15:19_

@zanieb 

From a quick search, I believe you can create a `impl ServerCertVerifier for CustomCertVerifier` where you check for a list of allowed hosts from the command line and skip the validation of TLS certificate at that moment


    let mut client_config = ClientConfig::builder()
        .with_custom_certificate_verifier(Arc::new(CustomCertVerifier {
            allowed_hosts: vec!["foo.com".into(), "bar.com".into()],
        }))

    let client = Client::builder()
        .use_preconfigured_tls(client_config)
        .build()?;



---

_Comment by @bashirmindee on 2024-04-23 13:25_

I am trying to use uv in a github workflow, and I am getting an error:
```
urllib.error.HTTPError: HTTP Error 403: SSL is required
```
seems to be related to this Issue. 
It seems that I need to use --trusted-host to solve my problem according to this [stackoverflow response](https://stackoverflow.com/questions/25981703/pip-install-fails-with-connection-error-ssl-certificate-verify-failed-certi)

---

_Comment by @brks-rssll on 2024-05-23 06:17_

What is the current best workaround?

---

_Comment by @inoa-jboliveira on 2024-05-23 11:10_

> What is the current best workaround?

To still use pip instead of uv. Sadly this is the major blocker for us

---

_Comment by @zanieb on 2024-05-23 13:04_

I'd accept a pull request adding this.

---

_Label `help wanted` added by @zanieb on 2024-05-23 13:04_

---

_Comment by @SoundDesignerToBe on 2024-05-29 13:41_

+1

---

_Comment by @fkapsahili on 2024-06-20 13:45_

Unfortunately, I also need this feature - I'll try to add this in a PR.

---

_Comment by @aldmbmtl on 2024-07-01 05:24_

This is also currently a blocking feature that we need at our company. We LOVE uv and use it for a ton of our docker builds, but we have private devpi servers that we launch for testing on CI and uv won't install from them sadly. 

I would happily submit a PR, but I don't know rust :(

---

_Comment by @fkapsahili on 2024-07-02 13:36_

> This is also currently a blocking feature that we need at our company. We LOVE uv and use it for a ton of our docker builds, but we have private devpi servers that we launch for testing on CI and uv won't install from them sadly.
> 
> I would happily submit a PR, but I don't know rust :(

I'm working on it currently, but it might take another week before I can submit a reviewable PR because it's more effort than I originally thought, and I only have a bit of experience with Rust, but I'm trying my best 🙂.

---

_Comment by @zanieb on 2024-07-02 14:25_

@fkapsahili Feel free to put up a draft early if you need help!

---

_Comment by @jreivilo on 2024-07-17 07:28_

> This is also currently a blocking feature that we need at our company. We LOVE uv and use it for a ton of our docker builds, but we have private devpi servers that we launch for testing on CI and uv won't install from them sadly.
> 
> I would happily submit a PR, but I don't know rust :(

Have you tried setting the environment variable `UV_NATIVE_TLS` to `true` in your Docker files? 
It worked for me :)

---

_Comment by @svart on 2024-08-02 05:47_

If you have root certificates for your registry installed in your system the workaround could be putting next lines in your `~/.config/uv/uv.toml`

```toml
native-tls = true

[pip]
index-url = "REGISTRY PATH"
```

After this all commands work well.

---

_Comment by @zmeir on 2024-08-19 10:35_

+1

This would make it much easier to migrate existing build pipelines that currently use `pip install` to instead use `uv pip install` without any changes to the requirements files being installed.

---

_Comment by @appleparan on 2024-08-20 12:37_

If the your CA isn't working even you generated CA by command like `update-ca-certificates', please check [this issue](https://github.com/astral-sh/uv/issues/5448) and try adjusting the permissions.

I resolved the certificate issue that persisted even after setting `UV_NATIVE_TLS=1` by running the following commands:

```
sudo chmod og=r /usr/local/share/ca-certificates/YOUR_CERTIFICATE.crt
```

then

```
UV_NO_CACHE=1 UV_NATIVE_TLS=1 uv install ...
UV_NO_CACHE=1 UV_NATIVE_TLS=1 rye sync
```



---

_Comment by @hbjydev on 2024-08-21 12:39_

Hi hi all,

I could do with having this feature. I run a personal PyPI repo on my network but didn't want to bother with setting up TLS for it, so I need to pass `--trusted-host` to `pip` in order to make it work. This basically blocks me from trying out `uv` currently.

---

_Comment by @gaby on 2024-08-22 11:52_

@charliermarsh @zanieb Does the astral team have an ETA for this? It's a huge blocker for the adoption of `uv` in offline, enterprise and CI environments. The PR https://github.com/astral-sh/uv/pull/4944 would had solved this but was closed.

---

_Comment by @zanieb on 2024-08-22 15:32_

https://github.com/astral-sh/uv/pull/4944 was not an acceptable solution, unfortunately. If someone is willing to investigate the solution described in the discussion there, we'll review it. Otherwise, we'll get to this when we can — we have a lot on our plate.

We generally don't provide ETAs for features. Please just 👍  the OP if you want this feature, don't ping everyone following the thread asking for an update.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-24 21:51_

---

_Comment by @charliermarsh on 2024-08-24 21:51_

PR open here: https://github.com/astral-sh/uv/pull/6591.

Anyone able to test this, or have advice on how to test this on macOS? :)

---

_Comment by @charliermarsh on 2024-08-24 22:58_

(Figured out a test workflow + found a few things to fix before merging.)

---

_Comment by @gaby on 2024-08-25 00:20_

@charliermarsh The tests are only running against `macos-latest-xlarge` which is now ARM based (Apple M1). Here https://github.com/astral-sh/uv/blob/main/.github/workflows/ci.yml#L190

If you also want to test against x86 based MacOS, you have to have to use a different image. Ideally as another Test Workflow.. See runner images docs here: https://github.com/actions/runner-images?tab=readme-ov-file#available-images

These are x86 with MacOS-14: `macos-latest-large or macos-14-large`

---

_Comment by @charliermarsh on 2024-08-25 14:55_

One clarification for anyone that's been waiting on this: IIUC, this isn't necessary for `http://` URLs. Unlike pip, uv doesn't warn when you use an `http://` URL as an index, so `--trusted-host` would have no effect on `http://` URLs in uv. This is only applicable to `https://` URLs, e.g., with a self-signed certificate.

---

_Closed by @charliermarsh on 2024-08-27 13:36_

---

_Closed by @charliermarsh on 2024-08-27 13:36_

---

_Comment by @Zyantist on 2024-09-27 07:24_

While the new changes work fine if I use a uv.toml, I keep getting errors locally as well as in CI docker containers, when I run something like: 

```shell
- >
  uv pip install 
  --allow-insecure-host="${LOCAL_NETWORK_IP}" 
  --extra-index-url="https://__token__:${some_token}@${LOCAL_NETWORK_IP}/api/v4/.../package1_ID/.../simple"
  --extra-index-url="https://__token__:${some_token}@${LOCAL_NETWORK_IP}/api/v4/.../package2_ID/.../simple"
  package1 package2
```

I get this chain of errors:
```shell
error: Failed to download  `somepackage==someversion`
    Caused by: Failed to unzip wheel:  somepackage.whl
    Caused by: an upstream reader returned an error: io error occurred: HTTP status client error (404 Not Found) for url (the wheel url and a sha appended)
    Caused by: io error occurred: HTTP status client error (404 Not Found) for url (the wheel url and a sha appended)
  Caused by: HTTP status client error (404 Not Found) for url (the wheel url and a sha appended)
```

This does not happen if I install them one by one with only one extra index url provided at a time. 
However, I want to be able to install a wheel that has dependencies pointing to wheels that require different extra index urls. It works fine with a local uv.toml file. 

Is the syntax when using multiple extra-index-url's in the command line different than when I do the same with vanilla pip or is this a bug resp. not yet a feature?

---

_Comment by @zmeir on 2024-09-27 07:47_

@Zyantist did you try `--index-strategy=unsafe-best-match`?

https://docs.astral.sh/uv/reference/settings/#index-strategy

---

_Comment by @Zyantist on 2024-09-27 08:07_

@zmeir Thank you very much! That works like a charm, at least locally where I just tried. I will test it in CI, too and report if issues occur. 

Update 1:  First attempt in CI failed. There I installed a wheel with multiple extra index urls and I used the --pre flag and the --system flag  as I do not require a venv in a docker container. I will post more information later, when I figured out what the breaking difference is between my local attempt and in CI

---

_Comment by @Zyantist on 2024-10-11 19:00_

Sorry for the delayed update. I took some hours today to test whatever could be tested. I finally was able to conclude the difference were the tokens I used.

The reason my attempt on CI failed is that I use project-individual access tokens there, whereas I use a private token locally. 

Today I tried to use the access tokens locally and all attempts at installation fail except when I use an individual access token in a single extra index url of a package that does not depend on other private packages that also require extra index url. 
Not-working example: install package A, that depends (per pyproject.toml) on project B and I provide two extra index urls with their individual tokens to install package A:  it fails. 

If I use my private token in both extra index urls, it works.

My private token has at lot more privileges than my access tokens. However, pip can work with these individual tokens, in CI and locally. 

For some reason, uv seems to either require tokens with more access rights (in complex situations) or it cannot handle multiple tokens in one installation command. 


Maybe this example extract of a toml helps to describe my situation:
```toml
# pyproject.toml of my-project-A
...
[project]
...
dependencies = [
    "public-project", 
    "my-project-B"
]

[project.optional-dependencies]
frontend = [
    "tkinter",
   "my-project-B[frontend]",  # nested tag, different token and different url
]
test = [
    "my-project-A[frontend]",   # recursive nested tag
    "pytest", 
]
```

This is what my setups look like but even a simpler one without the optional-dependencies fails to work with uv and individual tokens/urls. 


Are you aware of this different behaviour? 




---
