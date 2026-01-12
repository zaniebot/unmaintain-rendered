```yaml
number: 5726
title: Add PyPI cache to test runs
type: pull_request
state: closed
author: zanieb
labels:
  - help wanted
  - testing
assignees: []
draft: true
base: main
head: zb/ci-pypi-nginx
created_at: 2024-08-02T13:34:44Z
updated_at: 2024-10-01T22:44:37Z
url: https://github.com/astral-sh/uv/pull/5726
synced_at: 2026-01-12T16:06:59Z
```

# Add PyPI cache to test runs

---

_@zanieb_

Part of #5713 

---

_Label `testing` added by @zanieb on 2024-08-02 13:34_

---

_Comment by @zanieb on 2024-08-02 13:44_

Not hooked up yet, need to set the proxy variable.

Running into SSL errors locally.

---

_@samypr100 reviewed on 2024-08-02 23:27_

---

_Review comment by @samypr100 on `.github/workflows/ci.yml`:183 on 2024-08-02 23:27_

```suggestion
          export UV_INDEX_URL="http://localhost/simple"
```

Shouldn't be a need to specify the port in this case

---

_Comment by @blueraft on 2024-08-06 21:17_

Can I help here?

---

_Comment by @zanieb on 2024-08-06 21:23_

Feel free yeah. Though I'm not sure how we'll handle the test changes.

---

_Comment by @zanieb on 2024-08-06 21:23_

I presume using `ALL_PROXY` instead of `UV_INDEX_URL` will help, but not sure how to make https behave.

---

_Comment by @blueraft on 2024-08-06 21:26_

> Though I'm not sure how we'll handle the test changes.

 I was thinking of just filtering pypi/localhost url to [PYPI_SERVER].

> Running into SSL errors locally.

It works for me locally

---

_Comment by @zanieb on 2024-08-06 21:28_

I don't think that'll be sufficient, but it may be worth a try.

I run into SSL errors when using `HTTPS_PROXY`, not when using the index URL.

---

_Comment by @zanieb on 2024-08-06 21:30_

Before putting a lot of effort into snapshot correctness, you should probably see if this actually helps performance on the tests that _do_ pass.

---

_Comment by @blueraft on 2024-08-06 21:56_

```
❯ hyperfine -r 3 'cargo test -p uv --test pip_compile' 'UV_INDEX_URL=http://localhost/simple cargo test -p uv --test pip_compile'
Benchmark 1: cargo test -p uv --test pip_compile
  Time (mean ± σ):     29.200 s ±  1.987 s    [User: 76.855 s, System: 25.140 s]
  Range (min … max):   27.788 s … 31.472 s    3 runs

Benchmark 2: UV_INDEX_URL=http://localhost/simple cargo test -p uv --test pip_compile
  Time (mean ± σ):     23.838 s ±  0.740 s    [User: 77.656 s, System: 27.261 s]
  Range (min … max):   23.085 s … 24.566 s    3 runs

Summary
  UV_INDEX_URL=http://localhost/simple cargo test -p uv --test pip_compile ran
    1.22 ± 0.09 times faster than cargo test -p uv --test pip_compile
```

What do you think? 

---

_Comment by @zanieb on 2024-08-06 22:01_

I was hoping for much more than that, though it's still nice. I wonder where we're spending all of our time in that case?

---

_Comment by @blueraft on 2024-08-06 22:19_

The git dependencies seem to be the slowest to me, which could be cached too but that'd require some changes in the nginx server.

---

_Comment by @samypr100 on 2024-08-06 22:47_

> ```
> ❯ hyperfine -r 3 'cargo test -p uv --test pip_compile' 'UV_INDEX_URL=http://localhost/simple cargo test -p uv --test pip_compile'
> Benchmark 1: cargo test -p uv --test pip_compile
>   Time (mean ± σ):     29.200 s ±  1.987 s    [User: 76.855 s, System: 25.140 s]
>   Range (min … max):   27.788 s … 31.472 s    3 runs
> 
> Benchmark 2: UV_INDEX_URL=http://localhost/simple cargo test -p uv --test pip_compile
>   Time (mean ± σ):     23.838 s ±  0.740 s    [User: 77.656 s, System: 27.261 s]
>   Range (min … max):   23.085 s … 24.566 s    3 runs
> 
> Summary
>   UV_INDEX_URL=http://localhost/simple cargo test -p uv --test pip_compile ran
>     1.22 ± 0.09 times faster than cargo test -p uv --test pip_compile
> ```
> 
> What do you think?

Curious, was this after the requests got cached in nginx? (all nginx hits)

---

_Comment by @samypr100 on 2024-08-06 22:55_

> I don't think that'll be sufficient, but it may be worth a try.
> 
> I run into SSL errors when using `HTTPS_PROXY`, not when using the index URL.

Might be better to use an actual http proxy (e.g. squid) if we want to leverage `HTTPS_PROXY` / `HTTP_PROXY` env vars, although I haven't tried it. Will try to do some local tests sometime to see if it's any better than the nginx cache solution.

---

_Comment by @blueraft on 2024-08-07 10:12_

> Curious, was this after the requests got cached in nginx? (all nginx hits)

Yes tried it a few times.

> git dependencies seem to be the slowest to me

Local caching didn't help too much here.

```
 hyperfine 'uv pip install "flask @ git+http://localhost:1234/github.com/pallets/flask.git@735a4701d6d5e848241e7d7535db898efb62d400" --no-cache --reinstall' 'uv pip install "flask @ git+https://github.com/pallets/flask.git@735a4701d6d5e848241e7d7535db898efb62d400" --no-cache --reinstall'
Benchmark 1: uv pip install "flask @ git+http://localhost:1234/github.com/pallets/flask.git@735a4701d6d5e848241e7d7535db898efb62d400" --no-cache --reinstall
  Time (mean ± σ):      2.904 s ±  0.084 s    [User: 0.866 s, System: 0.411 s]
  Range (min … max):    2.804 s …  3.034 s    10 runs

Benchmark 2: uv pip install "flask @ git+https://github.com/pallets/flask.git@735a4701d6d5e848241e7d7535db898efb62d400" --no-cache --reinstall
  Time (mean ± σ):      3.327 s ±  0.183 s    [User: 1.300 s, System: 0.581 s]
  Range (min … max):    3.148 s …  3.647 s    10 runs

Summary
  uv pip install "flask @ git+http://localhost:1234/github.com/pallets/flask.git@735a4701d6d5e848241e7d7535db898efb62d400" --no-cache --reinstall ran
    1.15 ± 0.07 times faster than uv pip install "flask @ git+https://github.com/pallets/flask.git@735a4701d6d5e848241e7d7535db898efb62d400" --no-cache --reinstall
```



---

_Comment by @zanieb on 2024-08-07 20:58_

FWIW I tried to setup squid today and it's non-trivial because you still need to have it support SSL, which requires a self-signed cert etc. Annoyingly the image at `ubuntu/squid` doesn't support SSL so you need a custom build.

---

_Comment by @zanieb on 2024-08-07 21:35_

And for posterity, I tried using Caddy and Nginx but both are not really designed for caching a forward proxy.

---

_Comment by @samypr100 on 2024-08-08 02:07_

Agreed, SSL support is still non-trivial in this situation for transparent proxy cases. We'd need an action that generates a set of certs for you and make them trusted in every job.

---

_Comment by @zanieb on 2024-08-08 02:21_

I used this one-liner for a self-signed cert

```
openssl req -x509 -out localhost.crt -keyout localhost.key \
  -newkey rsa:2048 -nodes -sha256 \
  -subj '/CN=localhost' -extensions EXT -config <( \
   printf "[dn]\nCN=localhost\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:localhost\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")
```

---

_Comment by @samypr100 on 2024-08-08 02:41_

> I used this one-liner for a self-signed cert
> 
> ```
> openssl req -x509 -out localhost.crt -keyout localhost.key \
>   -newkey rsa:2048 -nodes -sha256 \
>   -subj '/CN=localhost' -extensions EXT -config <( \
>    printf "[dn]\nCN=localhost\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:localhost\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")
> ```

Makes sense, I use [mkcert](https://github.com/FiloSottile/mkcert) these days which makes a Root CA for you as well and helps make specific certs for specific hosts (including client certs). This then allows you do transparent proxying with SSL interception as needed. At least locally it's great, CI maybe not.

---

_Label `help wanted` added by @zanieb on 2024-08-08 14:15_

---

_Review comment by @johnslavik on `.github/workflows/ci.yml`:183 on 2024-08-09 00:36_

```suggestion
          export UV_INDEX_URL="http://127.0.0.1:80/simple"
```
https://www.youtube.com/watch?v=98SYTvNw1kw

and with @samypr100 suggestion
```suggestion
          export UV_INDEX_URL="http://127.0.0.1/simple"
```


---

_@johnslavik requested changes on 2024-08-09 00:36_

---

_@matthiask reviewed on 2024-08-12 06:20_

---

_Review comment by @matthiask on `.github/workflows/ci.yml`:148 on 2024-08-12 06:20_

```suggestion
      pypi-cache:
```

Probably a typo?

---

_Comment by @eth3lbert on 2024-08-13 02:52_

Is disabling ssl verification an option?

---

_Comment by @zanieb on 2024-08-13 03:41_

Does that switch to using HTTP instead of HTTPS? I sort of don't think so, it just ignores bad certificates. 

---

_Comment by @eth3lbert on 2024-08-13 04:03_

> Does that switch to using HTTP instead of HTTPS? I sort of don't think so, it just ignores bad certificates.

I primarily focus on the git dependencies. Here are a couple of options I've been exploring lately:

1. Prepare a local repository pool and fetch from `file://` (requires URL replacement in `uv-git`).
    - Since we use `git fetch` instead of `git clone`, we cannot leverage the `--reference-if-able` option.
2. Prepare a local repository pool, host it locally with a lb and `git-http-backend`, then configure git with something similar to this:
```
[url "https://git.example.com/"]
insteadOf = https://github.com/
sslVerify = false
```
  This way, we don't need to make any further changes, and requests from `github.com` should be redirected to `git.example.com` transparently in the background. (You can even use url "file:///path-to-pool" if that's acceptable.)

3. Similar to option 2, but configure with `sslCAInfo = cert file` instead of `sslVerify = false`.

Note: I haven't yet tested options 2 and 3 with authentication-related scenarios. More research is needed.
Additional note: The `git clone` command doesn't respect url rewrite rules.



---

_Comment by @zanieb on 2024-10-01 22:44_

This continues to look quite challenging, though I think it would be very valuable I don't think there's a clear path forward for this pull request.

---

_Closed by @zanieb on 2024-10-01 22:44_

---
