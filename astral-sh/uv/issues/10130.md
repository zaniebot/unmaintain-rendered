```yaml
number: 10130
title: Is it possible to skip fetching git submodules for some repositories?
type: issue
state: open
author: MrNaif2018
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2024-12-23T23:53:47Z
updated_at: 2025-04-19T18:11:57Z
url: https://github.com/astral-sh/uv/issues/10130
synced_at: 2026-01-10T03:41:46Z
```

# Is it possible to skip fetching git submodules for some repositories?

---

_Issue opened by @MrNaif2018 on 2024-12-23 23:53_

When we have a dependency from a github repository, we have two options:
`git+https://github.com/owner/repo@commit
`
Or
`https://github.com/owner/repo/archive/commit.zip
`
Using git is probably preferred, but it does submodule update at the beginning, and quite often this isn't needed for package building, and sometimes packages even depend on enterprise closed-source packages in their `.gitmodules`, making git option impossible
Would it be possible to add it to `[tool.uv.sources]`, something like
```
[tool.uv.sources]
httpx = { git = "https://github.com/owner/repo", gitmodules=false }
```
(Another thing to add could be a flag to enable shallow git clone (--depth=1) for example?)

The second approach is to use URL, which doesn't fetch full git repo history and can navigate to subdirectories too
It works great and we fixed some cases in it recently, but here github gives us the bummer: it sets max-age=0 on any kind of archive download I could find (api, regular urls, anywhere). So it means that uv spends some time (600ms for me with 1-2 git repos) on each uv sync/uv run invocation. I know I can probably disable sync, but it would be ideal to instead somehow mark those URL dependencies as excluded from sync or something?

In any case, any of those 2 improvements would greatly speed up our workflow as it relies on many pinned commits in different source repos (the owners of which don't want to publish to PyPI unfortunately)
Thanks in advance!

P.S. Just for context of some gitmodules log (I don't need any of them to build the package, they are for people who want to build GUI app for example, and even then people can install libraries by their own, but it takes forever to download and even fails)
```
--- stderr
      Submodule 'contrib/electrum-locale' (https://github.com/Electron-Cash/electrum-locale) registered for path 'contrib/electrum-locale'
      Submodule 'contrib/libevent' (https://github.com/libevent/libevent.git) registered for path 'contrib/libevent'
      Submodule 'contrib/openssl' (https://github.com/openssl/openssl.git) registered for path 'contrib/openssl'
      Submodule 'contrib/secp256k1' (https://github.com/Electron-Cash/secp256k1.git) registered for path 'contrib/secp256k1'
      Submodule 'contrib/tor' (https://github.com/EchterAgo/tor.git) registered for path 'contrib/tor'
      Submodule 'contrib/zbar' (https://github.com/mchehab/zbar) registered for path 'contrib/zbar'
      Submodule 'contrib/zlib' (https://github.com/madler/zlib.git) registered for path 'contrib/zlib'
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/93d858e1820fa8e2/6fc11cd9/contrib/electrum-locale'...
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/93d858e1820fa8e2/6fc11cd9/contrib/libevent'...
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/93d858e1820fa8e2/6fc11cd9/contrib/openssl'...
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/93d858e1820fa8e2/6fc11cd9/contrib/secp256k1'...
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/93d858e1820fa8e2/6fc11cd9/contrib/tor'...
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/93d858e1820fa8e2/6fc11cd9/contrib/zbar'...
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/93d858e1820fa8e2/6fc11cd9/contrib/zlib'...
      Submodule 'boringssl' (https://boringssl.googlesource.com/boringssl) registered for path 'contrib/openssl/boringssl'
      Submodule 'krb5' (https://github.com/krb5/krb5) registered for path 'contrib/openssl/krb5'
      Submodule 'pyca.cryptography' (https://github.com/pyca/cryptography.git) registered for path 'contrib/openssl/pyca-cryptography'
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/93d858e1820fa8e2/6fc11cd9/contrib/openssl/boringssl'...
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/93d858e1820fa8e2/6fc11cd9/contrib/openssl/krb5'...
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/93d858e1820fa8e2/6fc11cd9/contrib/openssl/pyca-cryptography'...
      Submodule 'src/ext/rust' (https://git.torproject.org/tor-rust-dependencies) registered for path 'contrib/tor/src/ext/rust'
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/93d858e1820fa8e2/6fc11cd9/contrib/tor/src/ext/rust'...
      fatal: unable to update url base from redirection:
        asked for: https://git.torproject.org/tor-rust-dependencies/info/refs?service=git-upload-pack
         redirect: https://gitlab.torproject.org/tpo/team
      fatal: clone of 'https://git.torproject.org/tor-rust-dependencies' into submodule path '/Users/alex/.cache/uv/git-v0/checkouts/93d858e1820fa8e2/6fc11cd9/contrib/tor/src/ext/rust' failed
      Failed to clone 'src/ext/rust'. Retry scheduled
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/93d858e1820fa8e2/6fc11cd9/contrib/tor/src/ext/rust'...
      fatal: unable to update url base from redirection:
        asked for: https://git.torproject.org/tor-rust-dependencies/info/refs?service=git-upload-pack
         redirect: https://gitlab.torproject.org/tpo/team
      fatal: clone of 'https://git.torproject.org/tor-rust-dependencies' into submodule path '/Users/alex/.cache/uv/git-v0/checkouts/93d858e1820fa8e2/6fc11cd9/contrib/tor/src/ext/rust' failed
      Failed to clone 'src/ext/rust' a second time, aborting
      fatal: Failed to recurse into submodule path 'contrib/tor'
```

---

_Label `configuration` added by @charliermarsh on 2024-12-26 14:39_

---

_Label `needs-decision` added by @charliermarsh on 2024-12-26 14:39_

---

_Comment by @MrNaif2018 on 2024-12-27 11:42_

@charliermarsh Sorry for ping! What is your stance on this feature? Technically I can try to open a PR to add gitmodules = false support as I know some Rust and I think it shouldn't be very complex. About second approach might be more complex though

---

_Comment by @charliermarsh on 2024-12-27 16:57_

I think something like this is reasonable (we've also tried to support shallow clones in the past), but it might be somewhat difficult to implement right now since we share a Git clone across packages. So we'd need some way to detect whether the repository was cloned with `gitmodules=false` or similar.

---

_Comment by @LeonarddeR on 2025-04-19 18:11_

When this is implemented, I'd also like to suggest listing specific submodules, e.g. `gitmodules = ["specific_mod"]`

---
