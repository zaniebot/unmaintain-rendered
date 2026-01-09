---
number: 6819
title: Multiple indexes without ssl verification
type: issue
state: open
author: jbevi
labels:
  - question
  - needs-mre
assignees: []
created_at: 2024-08-29T16:12:10Z
updated_at: 2025-07-31T15:30:06Z
url: https://github.com/astral-sh/uv/issues/6819
synced_at: 2026-01-07T13:12:17-06:00
---

# Multiple indexes without ssl verification

---

_Issue opened by @jbevi on 2024-08-29 16:12_

uv platform:  ubuntu 20
uv version: 0.4.0

Hello, I tell you my case.
I currently work with pipenv as a manager of virtual environments in projects.

In my company there is a pypi proxy and other indexes that the different areas use to upload their libraries.

With pipenv I can configure the indexes where to search for the project's libraries and disable ssl validation, as follows.

```
[pipenv]
install_search_all_sources = true

[[source]]
url = "https://nexus.cloudint.com/nexus/repository/pypi-proxy/simple"
verify_ssl = false
name = "pypi-proxy"

[[source]]
url = "https://nexus.cloudint.comr/nexus/repository/infraestructura-pypi/simple"
verify_ssl = false
name = "infra"

[packages]
fastapi = "==0.111.0"
pyownsecurity = {version = "==2.0.6", index = "infra"}
...
```

When installing the libraries of a project by doing: pipenv install

It is capable of installing them by searching the configured indexes.

Is there something similar in uv, for example configuring pyproject.toml ?
I have tried adding:

```
[tool.uv]
index-url = "https://nexus.cloudint.com/nexus/repository/pypi-proxy/simple/"
extra-index-url = ["https://nexus.cloudint.com/nexus/repository/infrastructure-pypi/simple/"]
```

When doing **uv add uvicorn** for example, it tries to search for the library in the extra-index-url, but not in index-url, which is where it is located, and it gives an ssl validation error. Log extract:

retrying: error sending request for url (https://nexus.cloudint.com/nexus/repository/infraestructura-pypi/simple/uvicorn/)
         Caused by: client error (Connect)
         Caused by: received fatal alert: HandshakeFailure
error: Request failed after 3 retries
  Caused by: error sending request for url (https://nexus.cloudint.com/nexus/repository/infraestructura-pypi/simple/uvicorn/)
  Caused by: client error (Connect)
  Caused by: received fatal alert: HandshakeFailure

Thanks in advance.

---

_Comment by @zanieb on 2024-08-29 16:18_

Hi! Do you mind reading these to see if they address your questions?

- https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes
- https://docs.astral.sh/uv/reference/settings/#index-strategy
- https://docs.astral.sh/uv/configuration/authentication/#custom-ca-certificates

---

_Label `question` added by @zanieb on 2024-08-29 16:18_

---

_Comment by @jbevi on 2024-08-29 17:42_

I tried adding to pyproject.toml:

[tool.uv]
allow-insecure-host = ["nexus.cloudint.com"]

but it seems to have no effect and the handshake error continues.

---

_Comment by @charliermarsh on 2024-08-29 17:47_

Can you run a command with `--show-settings`, and see if the setting is being picked up properly?

---

_Comment by @jbevi on 2024-08-29 18:05_

```
uv add uvicorn --show-settings
GlobalSettings {
    quiet: false,
    verbose: 0,
    color: Auto,
    native_tls: false,
    concurrency: Concurrency {
        downloads: 50,
        builds: 8,
        installs: 8,
    },
    connectivity: Online,
    show_settings: true,
    preview: Disabled,
    python_preference: Managed,
    python_downloads: Automatic,
    no_progress: false,
}
CacheSettings {
    no_cache: false,
    cache_dir: None,
}
AddSettings {
    locked: false,
    frozen: false,
    no_sync: false,
    packages: [
        "uvicorn",
    ],
    requirements: [],
    dependency_type: Production,
    editable: None,
    extras: [],
    raw_sources: false,
    rev: None,
    tag: None,
    branch: None,
    package: None,
    script: None,
    python: None,
    refresh: None(
        Timestamp(
            SystemTime {
                tv_sec: 1724954621,
                tv_nsec: 41840693,
            },
        ),
    ),
    settings: ResolverInstallerSettings {
        index_locations: IndexLocations {
            index: None,
            extra_index: [
                Url(
                    VerbatimUrl {
                        url: Url {
                            scheme: "https",
                            cannot_be_a_base: false,
                            username: "",
                            password: None,
                            host: Some(
                                Domain(
                                    "nexus.cloudint.com",
                                ),
                            ),
                            port: None,
                            path: "/nexus/repository/pypi-proxy/simple",
                            query: None,
                            fragment: None,
                        },
                        given: Some(
                            "https://nexus.cloudint.com/nexus/repository/pypi-proxy/simple",
                        ),
                    },
                ),
                Url(
                    VerbatimUrl {
                        url: Url {
                            scheme: "https",
                            cannot_be_a_base: false,
                            username: "",
                            password: None,
                            host: Some(
                                Domain(
                                    "nexus.cloudint.com",
                                ),
                            ),
                            port: None,
                            path: "/nexus/repository/infraestructura-pypi/simple/",
                            query: None,
                            fragment: None,
                        },
                        given: Some(
                            "https://nexus.cloudint.com/nexus/repository/infraestructura-pypi/simple/",
                        ),
                    },
                ),
            ],
            flat_index: [],
            no_index: false,
        },
        index_strategy: FirstIndex,
        keyring_provider: Disabled,
        allow_insecure_host: [
            TrustedHost {
                scheme: Some(
                    "https",
                ),
                host: "nexus.cloudint.com",
                port: None,
            },
        ],
        resolution: Highest,
        prerelease: IfNecessaryOrExplicit,
        config_setting: ConfigSettings(
            {},
        ),
        no_build_isolation: false,
        no_build_isolation_package: [],
        exclude_newer: None,
        link_mode: Hardlink,
        compile_bytecode: false,
        sources: Enabled,
        upgrade: None,
        reinstall: None,
        build_options: BuildOptions {
            no_binary: None,
            no_build: None,
        },
    },
}

```


---

_Comment by @zanieb on 2024-08-29 18:08_

Interesting, that all looks correct.

---

_Comment by @charliermarsh on 2024-08-29 19:35_

I suspect that your server is misconfigured in some other way... When the certificate is invalid, you tend to get `invalid peer certificate`?

---

_Comment by @djc on 2024-08-30 13:30_

Do you know what software is being used to run the PyPI proxy? I tried to connect to `nexus.cloudint.com` but I suppose that it is internal only. A packet capture might help a little but we'd probably need to setup `SSLKEYLOGFILE` support which I guess `uv` might not support out of the box.

---

_Comment by @ctz on 2024-08-30 13:32_

This is unlikely to be related to the server certificate. Instead, the server does not have any overlap with the client's TLS settings, so it sends a TLS alert saying a handshake is impossible. That is the "received fatal alert: HandshakeFailure" output; the client doesn't get any more information than this.

Suggest running a local tool like `sslscan` against the host to see what TLS versions and cipher suites it does support. 

---

_Comment by @jbevi on 2024-08-30 14:02_

Thanks for the quick responses.
I don't have the option to run sslscan on the server.
My main doubt is why this error occurs with UV, but not with pipenv, pip, or even curl.

---

_Comment by @djc on 2024-08-30 14:03_

No, you'd run `sslscan` on the client, against the server.

---

_Comment by @jbevi on 2024-08-30 14:14_

I'm not allowed to do that.
But I can run curl using the index url:

`curl https://nexus.cloudint.com/nexus/repository/pypi-proxy/simple/uvicorn/ -v`

I write the log extracts:

```
...
Connected to nexus.cloudint.com port 443 (#0)
...
SSL connection using TLSv1.2
...
HTTP/1.1 200 OK
...
<html lang="en">
<head><title>Links for uvicor</title>
  <meta name="api-version" value="2"/>
</head>
<body><h1>Links for uvicor</h1>
        <a href="../../packages/uvicorn/0.0.1/uvicorn-0.0.1.tar.gz#sha256=a88750101a094e293ed543f750e78be9fa00594966d32a7805b9d93fa0f9cdd5" rel="internal">uvicorn-0.0.1.tar.gz</a><br/>
....

```

---

_Label `needs-mre` added by @zanieb on 2024-10-21 21:39_

---

_Comment by @jbevi on 2024-11-13 13:28_

With pipenv I can disable ssl verification with verify_ssl = false on each index (see configuration in the first comment), is it possible to do this in uv?

---

_Comment by @charliermarsh on 2024-11-13 13:52_

You can add each URL to [`allow-insecure-host`](https://docs.astral.sh/uv/reference/settings/#allow-insecure-host). It's not a property of the index because it applies to arbitrary URLs, not just indexes.

---

_Comment by @jbevi on 2024-11-13 18:46_


Unfortunately that didn't work (see previous comments). Neither in v0.5.1.

---

_Comment by @jbevi on 2025-04-04 13:52_

Hi, has there been some progress on this issue? Still, with the latest version of uv, I can't contact the internal index service without certificates.
(I haven't tried doing it with certificates either.)

---

_Comment by @zanieb on 2025-04-04 17:04_

As a note, we could probably expose this option in the `[index]` API if we wanted, but it'd be good to see more need for it first since it's generally discouraged. cc @jtfmumm for visibility.

@jbevi I don't know what's wrong with your setup; unfortunately it's not something we can make progress on without a reproduction to debug or more details (as djc requested). I'd recommend setting up certificates if that's an option? Disabling SSL should not be a long-term strategy.



---

_Comment by @dgethings on 2025-04-25 18:25_

I'm facing the same (similar?) issue. I've heavily redacted the following output to spare the ~guilty~ innocent. I'm not SSL expert but I think the `sslscan` output is telling me there is no valid ciphers (from the server) which is why this is failing. But given I have `allow-insecure-host` set this should not be a problem.

```bash
❯ uv add "typer[all]"
⠴ my-app==0.1.0                                                                                                       
error: Failed to fetch: https://xxx/pypi/pypi-all/simple/atlassian-python-api/
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://xxx/pypi/pypi-all/simple/typer/)
  Caused by: client error (Connect)
  Caused by: received fatal alert: HandshakeFailure
 
❯ uv add "typer[all]" --show-settings
GlobalSettings {
    required_version: None,
    quiet: 0,
    verbose: 0,
    color: Auto,
    network_settings: NetworkSettings {
        connectivity: Online,
        native_tls: false,
        allow_insecure_host: [
            Host {
                scheme: None,
                host: "xxx",
                port: None,
            },
            Host {
                scheme: Some(
                    "https",
                ),
                host: "xxx",
                port: None,
            },
            Host {
                scheme: None,
                host: "xxx",
                port: Some(
                    443,
                ),
            },
            Host {
                scheme: Some(
                    "https",
                ),
                host: "xxx",
                port: Some(
                    443,
                ),
            },
        ],
    },
    concurrency: Concurrency {
        downloads: 50,
        builds: 11,
        installs: 11,
    },
    show_settings: true,
    preview: Disabled,
    python_preference: Managed,
    python_downloads: Automatic,
    no_progress: false,
    installer_metadata: true,
}
CacheSettings {
    no_cache: false,
    cache_dir: None,
}
AddSettings {
    locked: false,
    frozen: false,
    active: None,
    no_sync: false,
    packages: [
        "typer[all]",
    ],
    requirements: [],
    constraints: [],
    marker: None,
    dependency_type: Production,
    editable: None,
    extras: [],
    raw_sources: false,
    rev: None,
    tag: None,
    branch: None,
    package: None,
    script: None,
    python: None,
    install_mirrors: PythonInstallMirrors {
        python_install_mirror: None,
        pypy_install_mirror: None,
    },
    refresh: None(
        Timestamp(
            SystemTime {
                tv_sec: 1745583901,
                tv_nsec: 690955000,
            },
        ),
    ),
    indexes: [],
    settings: ResolverInstallerSettings {
        resolver: ResolverSettings {
            build_options: BuildOptions {
                no_binary: None,
                no_build: None,
            },
            config_setting: ConfigSettings(
                {},
            ),
            dependency_metadata: DependencyMetadata(
                {},
            ),
            exclude_newer: None,
            fork_strategy: RequiresPython,
            index_locations: IndexLocations {
                indexes: [
                    Index {
                        name: Some(
                            IndexName(
                                "xxx",
                            ),
                        ),
                        url: Url(
                            VerbatimUrl {
                                url: Url {
                                    scheme: "https",
                                    cannot_be_a_base: false,
                                    username: "me",
                                    password: Some(
                                        "password",
                                    ),
                                    host: Some(
                                        Domain(
                                            "xxx",
                                        ),
                                    ),
                                    port: None,
                                    path: "/pypi/pypi-all/simple",
                                    query: None,
                                    fragment: None,
                                },
                                given: Some(
                                    https://me:password@xxx/pypi/pypi-all/simple,
                                ),
                            },
                        ),
                        explicit: false,
                        default: true,
                        origin: None,
                        format: Simple,
                        publish_url: None,
                        authenticate: Auto,
                    },
                ],
                flat_index: [],
                no_index: false,
            },
            index_strategy: FirstIndex,
            keyring_provider: Disabled,
            link_mode: Clone,
            no_build_isolation: false,
            no_build_isolation_package: [],
            prerelease: IfNecessaryOrExplicit,
            resolution: Highest,
            sources: Enabled,
            upgrade: None,
        },
        compile_bytecode: false,
        reinstall: None,
    },
}
❯ sslscan --certs=/path/to/pem  https://xxx/
Version: 2.1.6
OpenSSL 3.4.1 11 Feb 2025
 
Connected to xxx
 
Testing SSL server xxx on port 443 using SNI name xxx
 
  SSL/TLS Protocols:
SSLv2     disabled
SSLv3     disabled
TLSv1.0   enabled
TLSv1.1   enabled
TLSv1.2   enabled
TLSv1.3   disabled
 
  TLS Fallback SCSV:
    Private key does not match certificate.
  TLS renegotiation:
    Private key does not match certificate.
Session renegotiation not supported
 
  TLS Compression:
    Private key does not match certificate.
  Heartbleed:
TLSv1.2 not vulnerable to heartbleed
TLSv1.1 not vulnerable to heartbleed
TLSv1.0 not vulnerable to heartbleed
 
  Supported Server Cipher(s):
    Private key does not match certificate.
    Private key does not match certificate.
    Private key does not match certificate.
 
  Server Key Exchange Group(s):
TLSv1.2  128 bits  secp256r1 (NIST P-256)
TLSv1.2  192 bits  secp384r1 (NIST P-384)
TLSv1.2  128 bits  x25519
    Private key does not match certificate.
    Private key does not match certificate.
 
```

---

_Comment by @dgethings on 2025-05-25 22:38_

@zanieb @jtfmumm I've included output from `sslscan` which I replicates the issue. Does that help move this issue forward?

---

_Comment by @zanieb on 2025-07-31 12:22_

Unfortunately the problem is probably upstream of us, in the Rust network stack. There's not a great way for us to dig into this ourselves.

---

_Comment by @djc on 2025-07-31 14:02_

If we can pare this down to a reproducer outside of uv we can probably find a way to make progress.

---

_Comment by @zanieb on 2025-07-31 14:56_

I hacked together a minimal project with an LLM, let's see if you can reproduce with that @dgethings? https://github.com/zanieb/uv-insecure-host-mre

---

_Comment by @ctz on 2025-07-31 15:07_

> I'm facing the same (similar?) issue. I've heavily redacted the following output to spare the ~guilty~ innocent. I'm not SSL expert but I think the `sslscan` output is telling me there is no valid ciphers (from the server) which is why this is failing.

The "`Private key does not match certificate`" output is saying you have supplied client authentication certificates, but no private key.  If the host needs client auth then you need to provide the private key with the `--pk` option. Otherwise, omit the `--certs` parameter entirely.

>But given I have `allow-insecure-host` set this should not be a problem.

`allow-insecure-host` disables verification of the server's certificate by the client. A server that -- for example -- only supports obsolete TLS versions won't get that far. This is what I suspect is the case here.

>   Caused by: received fatal alert: HandshakeFailure

This is the first message received from the server and terminates the handshake. So there is no opportunity for `allow-insecure-host` to have any effect.

>   Supported Server Cipher(s):
>     Private key does not match certificate.
>     Private key does not match certificate.
>     Private key does not match certificate.

This is where the interesting parts will be once sslscan works.

---

_Comment by @zanieb on 2025-07-31 15:30_

Thank you for the insights!

---
