```yaml
number: 6349
title: "Request for `uv.lock` to support different index urls across different developer machines and CI environments"
type: issue
state: open
author: humanzz
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2024-08-21T16:10:46Z
updated_at: 2025-12-18T15:46:49Z
url: https://github.com/astral-sh/uv/issues/6349
synced_at: 2026-01-10T03:11:31Z
```

# Request for `uv.lock` to support different index urls across different developer machines and CI environments

---

_Issue opened by @humanzz on 2024-08-21 16:10_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hello team,

Many thanks for the awesome work you're doing, and the countless amounts of time you've saved everyone using `uv` (and `ruff`).

I've seen the recent blog post about 0.3.0 and the accompanying documentation update, and from playing a bit to test commands like `uv lock` and `uv sync`, I actually have a question about `uv.lock`.

I've described the setup I use in https://github.com/astral-sh/uv/issues/1710 (a proxy, and using `invoke`, etc.). That setup means that on different team member's machine's, each is likely to have a different `INDEX_URL` and even the CI/CD system we use would have a different one.

I've generated the `uv.lock` file using `UV_INDEX_URL=... uv lock`, and the main thing I noticed is that the index url is included in the lock file e.g.

```
[[package]]
name = "aws-embedded-metrics"
version = "3.2.0"
source = { registry = "http://<omitted>/pypi/simple" }
dependencies = [
    { name = "aiohttp" },
]
sdist = { url = "http://<omitted>/pypi/simple/aws-embedded-metrics/3.2.0/aws-embedded-metrics-3.2.0.tar.gz", hash = "sha256:f235f87ab25ff328f6f3afca1c6b3218e81eea6e96e6aee012d368bb813fae7b" }
wheels = [
    { url = "http://<omitted>/pypi/simple/aws-embedded-metrics/3.2.0/aws_embedded_metrics-3.2.0-py3-none-any.whl", hash = "sha256:887b76d24914efa5fc42a7b77983e77fc670633e6e1195aac7653c425fee7399" },
]
```

I have not tried using that file across our different machines yet, but I suspect it's going to cause problems so I wanted to confirm with you folks
- What would happen if the index in the lock file is not available, and `UV_INDEX_URL`?
- Is there a way to not include the full index url in the lock file?


---

_Comment by @humanzz on 2024-08-24 12:12_

I've tested it further and confirmed that `uv sync` would fail on the different machines given the `UV_INDEX_URL=` would be pointing to a different index url.

With that that, I think this is a request to enable not writing the index url into the lock files via `uv lock`, and to enable `uv sync` - and other commands to use the lock file that does not have index urls as part of the lockfile

and to be more specific, this is about
- `source.registry`
- `sdist.url`
- `wheels.url`

fields

---

_Renamed from "Question about `uv.lock`, custom index urls, and its usage by the different commands" to "Request to enable `uv.lock` to not include the index url" by @humanzz on 2024-08-24 12:14_

---

_Label `wish` added by @charliermarsh on 2024-08-26 00:22_

---

_Comment by @mgab on 2024-09-02 10:01_

I discovered `uv` recently and I'm pretty excited about it! So even if we're discussing issues, thanks! 🫶

Now, coming to the topic, I think this becomes important given the ability to setup `index-url` and `extra-index-url` as user-level configuration, and potentially as system-level configuration (#6742).

Right now if you have a user-level config pointing at a package repository that acts as proxy to PyPI (that might add private pacakges on top) you will record that url in any lock file you generate. So if you want to work on some public project where `uv` is used as pacakge manager, either

1. the project -that you might not control- specifically defines `index-url` and `extra-index-url` to point at `PyPI`
2. you disable the user-level config 
3. or you will generate lock files that people outside of your org will not be able to use

Option 2 might not seem that bad, but then that defies the purpose of being able to define user-level or system-level configs. Plus, it might not be the ideal setup ideal to take advantage of `uvx` and `uv tool` using private packages that are only available in the private repository.

---

_Comment by @humanzz on 2024-09-04 10:54_

Just adding a couple more comments
- I did see on some other issue/pr, a mention that thanks to including those full paths, `uv` wouldn't be doing any further resolutions, and it just goes ahead to install things - which would save some time which sounds great
- In the case of the setup we're using, I noticed something which might be useful to call out
  - when the index, is public pypi index i.e. `https://pypi.org/simple`, the package artifacts have `url`s with a prefix looking like `https://files.pythonhosted.org/packages/c1/08/1bb1f7392cd9c3a65c4571a7f7286056ec52b3cd3d432485ca2b9907e0f9/`. For example, for `aws-embedded-metrics`, it's `https://files.pythonhosted.org/packages/c1/08/1bb1f7392cd9c3a65c4571a7f7286056ec52b3cd3d432485ca2b9907e0f9/aws-embedded-metrics-3.2.0.tar.gz`
  - in my case, this custom index, which uses a local proxy on dev/ci machines i.e. `http://<omitted>/pypi/simple`, the package srtifacts `url`s follow a more deterministic url pattern e.g. `http://<omitted>/pypi/simple/<package name>/<package version>/`. The same `aws-embedded-metrics` would be `http://<omitted>/pypi/simple/aws-embedded-metrics/3.2.0/aws-embedded-metrics-3.2.0.tar.gz`.

This determinism, can actually lead to a simpler handling of this, where I tested with
1. Generate the `uv.lock` with setting a `UV_INDEX_URL` to my custom index
1. Used it in other machines, by running another small script, prior to `uv sync`, that modifies `uv.lock` to replace the index in `http://<omitted>/pypi/simple` with the content of `UV_INDEX_URL`

All of this, makes me wonder if there's some solution to cases like mine, along the lines of
- Executing commands like `uv lock`, to have a flag to indicate usage of placeholder index urls, where it would have a place holder for `UV_INDEX_URL` or the extra indices
- Executing commands like `uv sync` can take a similar flag, where it'd use the values passed through `UV_INDEX_URL` to resolve the placeholder values in `uv.lock`



---

_Comment by @smheidrich on 2024-09-21 00:14_

What follows is a +1 comment you should all ignore, but I'm writing it to include some keywords in this issue so other people who run into this can find it more easily than I just did (took me a while):

-----

Having the same issue because I fetch all my PyPI packages through a local [devpi](https://github.com/devpi/devpi)<sup>(keyword&nbsp;1)</sup> installation, so all `source.registry` and `sdist.url` URLs in `uv.lock` say `http://localhost:3141/...`<sup>(keyword&nbsp;2)</sup> and, needless to say, this results in `Connection refused`<sup>(keyword&nbsp;3)</sup> errors when trying to install the package elsewhere, e.g. CI systems such as [GitHub Actions](https://github.com/smheidrich/demo-sphinx-paramlinks-sphinx-autoapi-compat/actions/runs/10967757294/job/30458100347)<sup>(keyword&nbsp;4)</sup> or GitLab CI/CD<sup>(keyword&nbsp;5)</sup>.

---

_Comment by @humanzz on 2024-09-21 12:57_

The more comments this gets, the more I think there needs to be an option for the lock file to not include any index information.

This way, all those use cases that use local proxies, different on developer machines from those in CI environments, and the likelihood that those different proxies having different conventions for their asset (wheel/sdist) URLs, that a general simple solution to exclude those index/asset URLs would be very useful and enable them to adopt `uv.lock`

---

_Comment by @tapetersen on 2024-09-23 19:46_

We could really also use this to be able to have lock-files without disclosing internal urls.
If not in the sync file then an option to the `export` subcommand to export a new uv.lock but with this or other things omitted would also be a good alternative

---

_Comment by @charliermarsh on 2024-09-23 19:52_

I'm not sure that a lockfile without URLs is a good idea. One of the goals of the lockfile is to be hermetic: you can install from _just_ the lockfile without making any queries or relying on any external sources.

If you're going to omit source information like that, why not just export to `requirements.txt`?


---

_Comment by @tapetersen on 2024-09-23 20:10_

@charliermarsh That may very well work for our requirements but it looks like the uv lockfile is richer (all the extras are declared). Otherwise it would just be a convenience to not have someone outside (that don't have access to internal dns-names) being able to use the lock-file without having to regenerate it from the requirements.txt (pardon me if I've missed a quicker way to handle that).

Basically my thought was that the hashes should be enough to guarantee that it's indeed the same packages that are being installed even if the urll has to be found again.

That said if it doesn't feel like the right way to do it and the recommended way is to manage that with requirements.txt exports we'll try that and raise a more specific issue or discussion if a showstopper or question regarding that comes up.

(feel free to edit/move/hide post if it derails original thread)

---

_Comment by @humanzz on 2024-09-23 20:19_

@charliermarsh 

At the moment, I'm actually using `uv pip compile pyproject.toml...` and `uv pip install -r ...` to simulate having a lockfile.

The main thing I don't like about it, is keeping the lockfile up to date with `pyproject.toml` dependencies is all manual. I didn't want to always call `uv pip compile`, then follow immediately by `uv pip install` as that would (1) be slower (2) force updating resolved dependencies that are declared via version ranges.

So, I'd say that's not on par with the experience of something like `package.json` and `package-lock.json` for node, and I was hoping that `uv.lock` and the commands that maintain/use it are giving a better experience.

The other thing I like about `uv.lock` is `uv.tool.dev-dependencies`. Without that, I end up declaring those dependencies as optional dependencies which is really not right, but ended up doing it this way, so I can declare all my dependencies in `pyproject.toml`, and then generate the current `requirements.txt` "lockfile".

Omitting the URLs is one potential solution - me thinking out loud.

In the case of my proxy, given that the `sdist`/`wheel` URLs have a specific pattern with the index url being a prefix, I was able to run some code to manipulate the lock file before running `uv sync --all-extras --frozen` to install dependencies

```python
@task
def update_lock_index_url(ctx):
    """
    Workaround for `uv lock` writing the index url to the lock file.
    Update `source` and `url` fields with values based on `UV_INDEX_URL` environment variable.
    """
    with open("uv.lock", "r+") as file:
        contents = file.read()
        pattern = r"http://127.0.0.1:\d+/[^/]+/[^/]+/[^/]+/pypi/simple"
        replacement = os.environ.get("UV_INDEX_URL", "NO_INDEX_OVERRIDE_SET")
        new_contents = re.sub(pattern, replacement, contents)
        file.seek(0)
        file.truncate()
        file.write(new_contents)
```

Downside for this, is that any dev on my team will end up having modifying `uv.lock`.
2 thoughts I had about this
1. `uv` having this concept of a hook - some python code to execute to modify file content - before installing dependencies
2. Ability to specify path to a lockfile, whereby I can run the hook myself, to generate a modified version of the lockfile, make sure it's in .gitignore, or in some tmp directory, and use `uv sync` while pointing to my modified version

I thought the simplest of all of the above would be this whole omission of URLs from the file tbh.
Moreover, generally when I think about what's uv story for supporting specifying index/extra index, and how that interacts with commands like `uv sync`, I think the strong assumption there is that the index URLs are not changing, which at least in my case is not true - whether across dev machines, or in the CI environment.

---

_Comment by @mgab on 2024-09-30 15:41_

> I'm not sure that a lockfile without URLs is a good idea. One of the goals of the lockfile is to be hermetic: you can install from just the lockfile without making any queries or relying on any external sources.
> 
> If you're going to omit source information like that, why not just export to requirements.txt?

As far as I can see, poetry records the file name and the hash, which should be enough to ensure that you are getting exactly the same files without relying on external sources.

On the other hand, by including the URLs you are coupling the lock file with the specific path of the package repository that you are using. And while this might remove the need of interacting with the package index, it also seems that adding a proxy package repository on the working environment should be transparent to the dependency specification of my project.

---

_Comment by @charliermarsh on 2024-10-03 19:43_

I consider it fairly critical that the lockfile includes the URLs directly and I suspect that the lockfile standard (should it be accepted) will do the same. We do have a plan to support these use-cases though. After #7481, we'll add something like:

```toml
[[tool.uv.index]]
name = "private"
url = "https://private.org/simple"
proxy = "http://<omitted>/pypi/simple"
```

So you can set the index URL (which will appear in the lockfile) and then the proxy separately. You'll also be able to set the proxy as an environment variable, like `UV_PRIVATE_PROXY_URL` or similar.

---

_Comment by @humanzz on 2024-10-04 08:44_

Hey @charliermarsh,

Just to make sure I understand this, and try to assess whether it'd work.
- The `url` in our case is not needed, though I dunno whether the intention is for it to be a valid index, or any url so maybe you can clarify that.
- I guess the `url` is what would be saved in index? and the `proxy` is kinda what replaces it when doing the actual resolving/downloads?
- for the package urls e.g. `sdist`, and `wheels`, how would that work with the `proxy`? In my earlier messages, I did see `https://pypi.org/simple` has `sdist` and `wheels` that don't have the index as part of their urls e.g. `https://files.pythonhosted.org/packages/c1/08/1bb1f7392cd9c3a65c4571a7f7286056ec52b3cd3d432485ca2b9907e0f9/aws-embedded-metrics-3.2.0.tar.gz` but in my case, it does have the index in the url e.g. `http://<omitted>/pypi/simple/aws-embedded-metrics/3.2.0/aws-embedded-metrics-3.2.0.tar.gz`


---

_Comment by @charliermarsh on 2024-10-04 10:45_

1. Ideally it is a valid index and indicative of the actual locations of the files but that wouldn't be strictly required.
2. Correct.
3. Both the file URLs and the registry URLs would be "replaced" correctly as long as they're on the same domain. So this wouldn't work for PyPI but would work for your case.

---

_Comment by @humanzz on 2024-10-05 13:04_

Great... then I look forwards to that :)

---

_Comment by @mgab on 2024-10-10 09:02_

> I consider it fairly critical that the lockfile includes the URLs directly and I suspect that the lockfile standard (should it be accepted) will do the same.

Sorry! Actually, reading about the proposed standard I found [this comment](https://discuss.python.org/t/pep-751-lock-files-again/59173/57) that properly describes what was my concern, and then the answer to it. The point that I was missing is in fact addressed in the [PEP draft](https://peps.python.org/pep-0751/#packages-files-url):

>  - Installers MUST NOT assume the URL will always work, but installers MAY use the URL if it happens to work.

So yeah, I understood that URLs would be assumed to work, and that didn't seem a great idea. I see that's not the case, so nothing to object!

What got me confused is that apparently `uv sync` does assume the URLs to work unless you explicitly configure a value for either `index-url` or `extra-index-url` somewhere. I reported it in #8076 and #8074, hope it's useful. In any case, that's just about keeping improving `uv` and not a design disagreement.

Thanks for the answers!

---

_Comment by @humanzz on 2024-10-17 16:15_

I saw that the revamped index support got released in 0.4.23.

Is there any separate issues to track the proxy/env variables support, or is this issue itself that?

---

_Comment by @charliermarsh on 2024-10-17 16:41_

I think it’s ok to track it here.

---

_Renamed from "Request to enable `uv.lock` to not include the index url" to "Request to support `uv.lock` to not include the index url" by @humanzz on 2024-10-28 11:46_

---

_Renamed from "Request to support `uv.lock` to not include the index url" to "Request for `uv.lock` to support different index urls across different developer machines and CI environments" by @humanzz on 2024-10-28 11:48_

---

_Comment by @rd-danny-fleer on 2024-12-02 09:23_

May I ask what's the current status of the issue? What aspects remain unclear?

Currently I use a workaround (Run `uv lock` once in the CICD pipeline with the alternative index URL and use the updated `uv.lock` file in the whole pipeline) to make CICD work with an alternative index. However, with this approach, you must be careful not to update any dependencies.

---

_Comment by @charliermarsh on 2024-12-02 13:30_

I don't know if anything is unclear, it's more that it hasn't been prioritized.

---

_Comment by @dariocurr on 2025-01-07 08:21_

Any updates on this? This is critical for us to ship `uv` in our pipelines

---

_Comment by @humanzz on 2025-02-10 14:38_

I know it's already February, but it would still make a great New Year's present! 😄

---

_Assigned to @jtfmumm by @jtfmumm on 2025-02-25 22:20_

---

_Comment by @jtfmumm on 2025-02-26 14:49_

I've [created a draft PR](https://github.com/astral-sh/uv/pull/11782) for this issue. The current design adds a new command line arg (`--index-proxy-url <index-name>=<proxy-url>`) and env var (`UV_INDEX_PROXY_URL=<index-name>=<proxy-url>`). 

You define a canonical URL for an index by defining the index the normal way in `pyproject.toml`:

```
 [[tool.uv.index]]
name = "private"
url = "https://private.org/simple"
```

When passing in a proxy URL for an index, that URL will be used for making calls to the registry. However, only the canonical URL will be written to the lockfile. If the canonical URL is also valid, then you can run commands like `uv sync` without a proxy URL. If it's not valid, you will need to pass in the proxy URL.

This is supported for `uv run`, `uv add`, `uv sync`, and `uv lock`.

One tricky aspect ([mentioned above](https://github.com/astral-sh/uv/issues/6349#issuecomment-2393167729)) is that this will only work if your index uses its URL as the base URL for the source distributions and wheels. So as Charlie [indicated](https://github.com/astral-sh/uv/issues/6349#issuecomment-2393409286), this doesn't work for PyPi, for example.

I'd be curious to hear any feedback about whether this addresses your concerns and whether the API makes sense. I'd also be curious if anyone would be interested in trying out my branch with their projects.


---

_Comment by @apollo13 on 2025-02-26 15:20_

Hi @jtfmumm, thank you for your work on this! Since you asked for feedback… ;)

> The current design adds a new command line arg (--index-proxy-url <index-name>=<proxy-url>) and env var (UV_INDEX_PROXY_URL=<index-name>=<proxy-url>).

This works only for a single index if I understand the code correctly? I guess this will work for the majority of cases and at least `--index-proxy-url` could be specified multiple times. Not sure how that would work for `UV_INDEX_PROXY_URL` though.

> One tricky aspect (https://github.com/astral-sh/uv/issues/6349#issuecomment-2393167729) is that this will only work if your index uses its URL as the base URL for the source distributions and wheels. So as Charlie https://github.com/astral-sh/uv/issues/6349#issuecomment-2393409286, this doesn't work for PyPI, for example.

That is the biggest downside for me. We usually try to mirror PyPI to save bandwidth (for PyPI). I wonder if supporting something like PDM has would make sense: https://pdm-project.org/latest/usage/lockfile/#static-urls -- PDM has an option to only store the filename in the lockfile which probably means more requests to the index at some point (?). 

---

_Comment by @jtfmumm on 2025-02-26 16:06_

Hi @apollo13, thanks for the feedback! 

Currently you can specify `--index-proxy-url` multiple times. We're going to change the env var to be of the form `UV_INDEX_<index-name>_PROXY_URL`. That is how we currently handle username and password env vars for indexes.

> PDM has an option to only store the filename in the lockfile

Charlie raises the concern [above](https://github.com/astral-sh/uv/issues/6349#issuecomment-2392197432) that URLs may be required once the lockfile standards are accepted, so I'm not sure if this option is open to us.

---

_Comment by @apollo13 on 2025-02-26 17:18_

> Charlie raises the concern [above](https://github.com/astral-sh/uv/issues/6349#issuecomment-2392197432) that URLs may be required once the lockfile standards are accepted, so I'm not sure if this option is open to us.

Understood, I wonder if we can come up with other creative ideas. Because using a proxy for PyPI in CI is the *one* situation where I see massive bandwidth savings for PyPI.

---

_Comment by @charliermarsh on 2025-02-26 17:45_

> Understood, I wonder if we can come up with other creative ideas. Because using a proxy for PyPI in CI is the one situation where I see massive bandwidth savings for PyPI.

When you proxy PyPI in this way, are you also proxying the returned file URLs? Or just the Simple index API? Either way, I think this should still be fine with the proposed API, since... if you're just using the file URLs returned by PyPI, they don't need authentication; and if you're proxying those, then in theory you can use a domain that's the same as the top-level Simple API domain.

---

_Comment by @apollo13 on 2025-02-26 17:58_

The returned URLs are rewritten. Ie it is not a simple HTTP proxy but a repository manager like sonatype nexus, resulting in this download link for a package:
```
https://my_host/repository/pypi-proxy/packages/packaging/24.2/packaging-24.2-py3-none-any.whl
```
which makes the URL pretty much different from PyPI which has it at:
```
https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl
```

---

_Comment by @charliermarsh on 2025-02-26 18:01_

But does `https://my_host/repository/pypi-proxy/packages/packaging/24.2/packaging-24.2-py3-none-any.whl` match the host of the registry itself, i.e., the thing you pass to `--index`?

---

_Comment by @apollo13 on 2025-02-26 18:07_

I guess so, ie the simple index URL for the package `packaging` would be:
```
https://my_host/repository/pypi-proxy/simple/packaging
```
(my_host being the same as above). But I still don't understand why you think this would work. If I use the proxy to create the lockfile it would write package urls ala `.../packaging/24.2/packaging-24.2-py3-none-any.whl` into the lockfile and I don't think replacing that to the canonical URL would work.

EDIT:// if we are going off-topic here just ping me on discord.

---

_Comment by @charliermarsh on 2025-02-26 18:17_

Ah I see. So you want to use a proxy for your own development, but put public URLs in the lockfile itself. That seems much more challenging, since we'd need to query PyPI for every package to get those URLs.

---

_Comment by @humanzz on 2025-03-03 21:41_

Hi @jtfmumm!
I have no clue how I missed this, but many thanks for starting to work on this.

In my case, the `UV_INDEX_URL` is set to the proxy url, that `uv` uses. The value of that env variable will different between individual developers machines, and also on CI/CD environment. This means I cannot use a named index and a corresponding proxy index via `UV_INDEX_<index-name>_PROXY_URL`. I need to be able to use both `UV_INDEX_URL` and `UV_INDEX_PROXY_URL`.

According to the description, if I understand this correctly, what is now set in `UV_INDEX_URL` should actually go to `UV_INDEX_PROXY_URL`, while `UV_INDEX_URL` should have the canonical url for use in lock file, correct?

---

_Comment by @xbeastx on 2025-05-14 07:43_

@charliermarsh @zanieb 
First of all, I want to thank you for building such an amazing tool as **uv**. It's genuinely fast — very impressive.

However, I’d like to draw attention to some ideological aspects of the `uv.lock` file format that are presented as universally correct.

It’s clear that the **primary purpose** of `uv.lock` is to ensure **reproducibility** of the package environment, so that behavior during testing and development remains stable. From an architectural standpoint, achieving this goal clearly requires controlling the list of packages being installed and their exact content — in other words, **what** is installed. That alone is entirely sufficient to guarantee consistent results.

But **where** exactly these packages are fetched from does not necessarily need to be part of the lock file. They could come from a local folder, an AWS S3 bucket, PyPI, or anywhere else.

In other words, the **source location** is more appropriately a property of the **installation process**, not the lock file itself. I suppose that including the URL might indeed help with "not making some queries", but it blurs the boundaries of responsibility.
That’s why we have to ask: is `uv.lock` meant to **simplify using index**, or to **lock down state**? From my point of view these are two very different goals.

That said, if some users really **want to lock with full URLs**, perhaps there could be two uv lock "modes":
- `default`: no URLs are saved,
- `source_locked`: URLs are saved.

For more complex cases, where a package comes from a non-default repository, the lock file could include the name and URL of the index for that specific package.

For example, a `uv.lock` might look like this:
```toml
[[source]]
name = "pytorch-cuda"
url = "https://download.pytorch.org/whl/cu121"

[[package]]
name = "requests"
version = "2.31.0"
sdist = { 
    hash = "sha256:abc123...",
    size = 120000,
    upload-time = "2024-06-01T10:00:00Z"
}

[[package]]
name = "torch"
version = "2.2.0+cu121"
source = { index = "pytorch-cuda" }
wheels = [
    {
        hash = "sha256:xyz789...",
        size = 250000000,
        upload-time = "2024-03-01T08:00:00Z"
    }
]
```
In this example, the `requests` package would use the default index (either as configured explicitly, via `UV_DEFAULT_INDEX`, or just fall back to PyPI), while the torch package would be fetched from the `pytorch-cuda` index as specified in the lock file.

With this design, it would also be easy to override sources at install time via environment variables or CLI options:
```bash
UV_DEFAULT_INDEX=https://internal-repo.com/ UV_INDEX=pytorch-cuda=https://another-repo.com/simple uv sync
```
In my opinion, this approach to using `uv.lock` would be both **architecturally sound** and **highly flexible**.

In today’s world — where almost every company uses local mirrors of PyPI enriched with some internal packages — this actively blocks adoption of `uv`. And just URL inside lock file looks really strange...

_P.S. And let’s just imagine for a moment that every developer starts committing `uv.lock` files into their repos with entries like:
`https://files.pythonhosted.org/packages/4d/f9/9a7ce600ebe7804daf90d4d48b1c0510a4561ddce43a596be46676f82343/...`
What happens if PyPI ever decides to change their domain name or URL format? Does that mean all existing lock files will break? Or at least we must have some fall-back logic to reacquire this URL and still "make some queries"?_

---

_Comment by @ngnpope on 2025-06-30 16:26_

I've bumped into this at work today, but with a slightly different scenario. We have a private repository and proxy/mirror for PyPI. These are provided for local development via a VPN _(on an "internal" URL)_ and via mTLS for CI _(on an "external" URL)_. When accessing over the VPN we don't need to provide credentials for the repositories, but we do when accessing in CI over the external URL.

A couple of options came to my mind initially regarding a solution for this:

1. Provide an environment variable for the hostname

    - This has the disadvantage that the environment variable would always have to be set, unless it were possible to provide a default, e.g. `${INDEX_HOST:-repo.example.net}`. This sort of shell-like expansion is supported, for example, in Dockerfile.
    - And because that would need to be copied for every URL in the lock file it would quickly get messy...
    - Would depend on #5734

2. Provide something like git's [`url.<base>.insteadOf`](https://git-scm.com/docs/git-config#Documentation/git-config.txt-urlbaseinsteadOf)

    - This would make it possible to drop a configuration file in CI providing a mapping of the base URL
    - It could also address the concerns around being able to provide a mapping for other URLs for source distributions and wheels
    - The downside is that it would be hard/messy to be able to do this via environment variables, so may only be limited to configuration files

I then discussed this with some colleagues and was helpfully pointed to this issue. Having looked through, it seems that Charlie's [original proposal](https://github.com/astral-sh/uv/issues/6349#issuecomment-2392197432) would have solved my issue, e.g. something like:

```toml
[[tool.uv.index]]
name = "mirror"
url = "https://repo.corp.net/mirror/simple"
default = true

[[tool.uv.index]]
name = "private"
url = "https://repo.corp.net/private/simple"
```

And then being able to override in CI with:

```sh
UV_MIRROR_PROXY_URL="https://repo.ext.corp.net/mirror/simple"
UV_PRIVATE_PROXY_URL="https://repo.ext.corp.net/private/simple"
```

As for my other suggestion number two above, that could look something like:

```toml
[[tool.uv.index.remap]]
"https://repo.corp.net/" = "https://repo.ext.corp.net/"

# We can even remap URLs for hosted files:
"https://files.pythonhosted.org/" = "https://some.other.mirror/"
```

(That's a rather rough example, but hopefully gets the point across.)

---

_Comment by @ArcticLampyrid on 2025-07-03 16:58_

**FYI, here’s a summary of how another ecosystem—Poetry—handles this:**

1. It does *not* include the concrete sdist/wheel URLs in the lockfile; it only uses the SHA‑256 checksum.
2. It records the package *source*, unless it’s using the official PyPI source. If no source is recorded, it always defaults to PyPI’s official source.
3. There’s a plugin called [poetry-plugin-pypi-mirror](https://github.com/arcesium/poetry-plugin-pypi-mirror) that can replace the default source with a mirror at the project or user level (this replacement does *not* end up in the lockfile).

---

**IMO**, sdist/wheel URLs are *not* permanent—they’re internal implementation details of PyPI. You should always fetch the latest URL from the index file. For reproducibility and integrity checks, using the SHA‑256 checksum alone is sufficient.

In ecosystems like NPM, URLs are recorded because they’re fully canonicalized:

```
https://registry.npmjs.org/typescript/-/typescript-5.4.3.tgz
```

But PyPI’s URLs are *not* like that; they look more like opaque object storage identifiers:

```
https://files.pythonhosted.org/packages/c1/08/1bb1f7392cd9c3a65c4571a7f7286056ec52b3cd3d432485ca2b9907e0f9/aws-embedded-metrics-3.2.0.tar.gz
```

---

_Comment by @charliermarsh on 2025-07-03 18:07_

I'm not a fan of omitting the URLs. It makes the installation protocol more complicated (it has to implement the Simple API), and what we're doing here is much more aligned with the standardized `pylock.toml` format from PEP 751 (which also records URLs). Unless I'm misunderstanding, omitting the file URLs doesn't actually solve the problem in this issue anyway, since we still need to record the source, which is what's being modulated?

(As an aside, PyPI's URLs are not exactly opaque identifiers; they're content-addressed locations. The segments comprise the Blake2 hash of the file.)


---

_Comment by @ArcticLampyrid on 2025-07-04 14:24_

> Unless I'm misunderstanding, omitting the file URLs doesn't actually solve the problem in this issue anyway, since we still need to record the source, which is what's being modulated?

Provide a option to add alternative URLs for sources are easy. But it's hard for sdist/wheel URLs: 

> In the case of the setup we're using, I noticed something which might be useful to call out
> * when the index, is public pypi index i.e. `https://pypi.org/simple`, the package artifacts have `url`s with a prefix looking like `https://files.pythonhosted.org/packages/c1/08/1bb1f7392cd9c3a65c4571a7f7286056ec52b3cd3d432485ca2b9907e0f9/`. For example, for `aws-embedded-metrics`, it's `https://files.pythonhosted.org/packages/c1/08/1bb1f7392cd9c3a65c4571a7f7286056ec52b3cd3d432485ca2b9907e0f9/aws-embedded-metrics-3.2.0.tar.gz`
> * in my case, this custom index, which uses a local proxy on dev/ci machines i.e. `http://<omitted>/pypi/simple`, the package srtifacts `url`s follow a more deterministic url pattern e.g. `http://<omitted>/pypi/simple/<package name>/<package version>/`. The same `aws-embedded-metrics` would be `http://<omitted>/pypi/simple/aws-embedded-metrics/3.2.0/aws-embedded-metrics-3.2.0.tar.gz`.



> As an aside, PyPI's URLs are not exactly opaque identifiers; they're content-addressed locations. The segments comprise the Blake2 hash of the file.

Thanks for information. But I am still curious whether this is a documented detail that can be relied upon.

---

_Comment by @ArcticLampyrid on 2025-07-04 14:28_

Adding a point, if the purpose of using a mirror is to bypass network censorship issues (which does happen in China due to [GFW](https://en.wikipedia.org/wiki/Great_Firewall)), it is unacceptable to obtain URL details from multiple sources at the same time. Accessing PyPI is likely to result in failure.

---

_Comment by @liblaf on 2025-07-04 16:21_

Hi everyone, this is a long-standing and important discussion with many thoughtful contributions. As a user who is keenly following this, I wanted to try to synthesize the core problem, the main points of debate, and the proposed solutions so far.

### The Core Problem

The current `uv.lock` file includes absolute URLs for:

- Package registries (`source.registry`)
- Source distributions (`sdist.url`)
- Wheel files (`wheels.url`)

This causes issues when:

1. Developers use different index URLs / proxies locally vs. in CI
2. Organizations have private proxies / mirrors with custom URL patterns
3. Lockfiles contain internal URLs that shouldn't be exposed
4. Environments can't access the originally locked URLs

### The Central Tension: Portability vs. Hermeticity

The discussion has highlighted a key design tension:

- **Portability & Separation of Concerns:** Some feel that a lockfile should define _what_ to install (package, version, hash) but that _where_ it's downloaded from is an installation-time detail. This makes the lockfile portable and keeps environment-specific configuration out of committed project files.
- **Hermeticity & Performance:** Some have emphasized the goal of a hermetic lockfile. Storing exact URLs means `uv sync` doesn't need to perform any index lookups; it can just download the files directly. This is faster, simpler for the installer, and aligns with the direction of the proposed `pylock.toml` standard (PEP 751).

---

Here are the main approaches discussed so far, as I see them:

#### Solution 1: Omit URLs from the lockfile (The "Poetry" / "Separation of Concerns" model)

- **Where it comes from:** This was one of the earliest suggestions by @humanzz and was advocated for by @xbeastx and @ArcticLampyrid. The core idea is that the lockfile should specify _what_ to install (package name, version, hash), while the environment should specify _where_ to get it from. @ArcticLampyrid pointed out that this is how **Poetry** operates.
- **Pros:**
  - **Maximum Portability:** The `uv.lock` file would be completely agnostic to the index/mirror setup and would work seamlessly across any environment.
  - **Simplicity:** Solves all the different use cases (local proxies, different CI URLs, public vs. private URLs) without complex configuration.
- **Cons:**
  - **Not Hermetic:** As @charliermarsh pointed out, this goes against the goal of having a lockfile that can be used for installation without any further network queries to an index API. `uv sync` would need to re-resolve file locations.
  - **Standard Alignment:** This approach seems to diverge from the proposed `pylock.toml` format (PEP 751), which includes URLs.

#### Solution 2: Canonical URL with a Proxy override (The current PR approach)

- **Where it comes from:** This was proposed by @charliermarsh and a draft PR was created by @jtfmumm. The idea is to define a canonical URL for an index in `pyproject.toml`, which gets written to the lockfile. A specific environment can then provide a "proxy" URL via an environment variable (`UV_INDEX_<name>_PROXY_URL`) or flag to use instead.
- **Pros:**
  - **Preserves Hermeticity:** The lockfile still contains valid, canonical URLs.
  - **Clear Intent:** Separates the project's official source from an environment-specific override.
- **Cons:**
  - **The PyPI Mirror Problem:** This is the biggest drawback, as highlighted by @apollo13. This approach only works if the file URLs (for wheels/sdists) are on the same domain as the index URL. It **may not work** for PyPI mirrors (like Nexus, Artifactory, or public mirrors) which rewrite `files.pythonhosted.org` URLs to their own domain. This seems to defeat one of the primary use cases for this feature.

#### Solution 3: Full URL Rewriting / Remapping (The "Git" / "Pixi" model)

- **Where it comes from:** This is a more powerful version of the proxying idea, inspired by `git`'s `url.<base>.insteadOf` (suggested by @ngnpope) and **pixi**'s `[mirrors]` configuration (from issue [#9056](https://github.com/astral-sh/uv/issues/9056)).
- **The Idea:** Allow arbitrary URL prefix remapping in the configuration. This would let a user map _both_ the index URL (`https://pypi.org/simple`) and the file host URL (`https://files.pythonhosted.org`) to their mirror equivalents.
- **Pros:**
  - **Solves the PyPI Mirror Problem:** This is flexible enough to handle the case where file URLs are on a different host from the simple index, making it compatible with most real-world proxy/mirror setups.
  - **Powerful & Flexible:** It can handle a wide variety of scenarios, including the complex internal / external URL mapping described by @ngnpope.
- **Cons:**
  - **Increased Complexity:** This is a more complex feature to design, implement, and for users to configure compared to the other options.

---

I've done my best to capture the nuances of this long discussion, but I may have missed or misinterpreted some points. Please feel free to correct me!

Perhaps we could do an informal poll using emoji reactions on this comment:

- **Solution 1: Omit URLs from the lockfile (The "Poetry" / "Separation of Concerns" model):** React with ❤️
- **Solution 2: Canonical URL with a Proxy override (The current PR approach):** React with 🚀
- **Solution 3: Full URL Rewriting / Remapping (The "Git" / "Pixi" model):** React with 👀
- **None of the above / More discussion needed:** React with 🤔

---

_Comment by @liblaf on 2025-07-16 05:01_

For anyone who need to use PyPI mirrors and don't want to pollute `uv.lock` with mirror URLs, here's a not-so-elegant workaround. Before each `uv` command, we replace the URLs in `uv.lock` with mirror URLs. After `uv` command is finished, the mirror links are replaced with the default `pypi.org` and `files.pythonhosted.org`.

This workaround only works for mirrors which maps original URLs in a pattern similar to:

```
https://pypi.org/simple
-> https://mirrors.cernet.edu.cn/pypi/web/simple
https://files.pythonhosted.org/packages/1d/56/f5d...8fe/liblaf_grapes-0.6.0.tar.gz
-> https://mirrors.cernet.edu.cn/pypi/web/packages/1d/56/f5d...8fe/liblaf_grapes-0.6.0.tar.gz
```

It works fine with CERNET MirrorZ, and should works with most other mirrors as well. Comments on compatible mirror sites or more elegant and general methods are welcome.

During my test, I found that `uv.lock` may miss a few, only a few `package[*]{sdist,wheels[*]}.{size,upload-time}`. Luckily, this doesn't affect functionality. Even if someone re-lock using origin PyPI index, it won't change `uv.lock`.

The following is a fish shell function that wraps `uv`. Similarly, we can implement alias / functions for other shells, such as bash, zsh, etc.

```fish
function uv --wraps uv
    # similar alias / functions can be implemented in other shells, e.g. bash, zsh
    set --local PYPI_MIRROR "https://mirrors.cernet.edu.cn/pypi/web"
    set --local git_root (git rev-parse --show-toplevel)
    set --local uv_lock "$git_root/uv.lock"
    echo "$uv_lock"
    if test -f $uv_lock
        # `sed` can also be used
        sd --fixed-strings 'https://pypi.org' "$PYPI_MIRROR" "$uv_lock"
        sd --fixed-strings 'https://files.pythonhosted.org' "$PYPI_MIRROR" "$uv_lock"
    end
    UV_DEFAULT_INDEX="$PYPI_MIRROR/simple" command uv "$argv"
    if test -f $uv_lock
        # since mirrorz-302 redirects you to your "best" mirror site, the URLs
        # in `uv.lock` does not have to be the same with `${PYPI_MIRROR}`
        sd 'https://[\.\w]+/pypi/web/simple' 'https://pypi.org/simple' "$uv_lock"
        sd 'https://[\.\w]+/pypi/web/packages' 'https://files.pythonhosted.org/packages' "$uv_lock"
    end
end
```

To use mirrors in [Pixi](https://pixi.sh/)-managed projects (which uses `uv` to handle PyPI packages), [a similar workaround](https://github.com/prefix-dev/pixi/issues/4147) can also be used.

---

_Comment by @waketzheng on 2025-07-23 13:52_

I replace the registry and url by the following script:
```py
#!/usr/bin/env python
"""Auto change registry of uv.lock to be pypi.org

Usage::
    python3 <me>.py
"""

from __future__ import annotations

import functools
import re
import sys
from pathlib import Path

PYPI = "https://pypi.org/simple"
HOST = "https://files.pythonhosted.org"


def main() -> int | None:
    verbose = "--verbose" in sys.argv
    p = Path("uv.lock")
    if not p.exists():
        p = Path("..", p.name)
        if not p.exists():
            if verbose:
                print(f"{p.name} not found, skip.")
            return None
    text = p.read_text("utf-8")
    registry_pattern = r'(registry = ")(.*?)"'
    replace_registry = functools.partial(re.sub, registry_pattern, rf'\1{PYPI}"')
    registry_urls = {i[1] for i in re.findall(registry_pattern, text)}
    download_pattern = r'(url = ")(https?://.*?)(/packages/.*?\.)(gz|whl)"'
    replace_host = functools.partial(re.sub, download_pattern, rf'\1{HOST}\3\4"')
    download_hosts = {i[1] for i in re.findall(download_pattern, text)}
    if not registry_urls:
        raise ValueError(f"Failed to find pattern {registry_pattern!r} in {p}")
    if len(registry_urls) == 1:
        current_registry = registry_urls.pop()
        if current_registry == PYPI:
            if download_hosts == {HOST}:
                if verbose:
                    print(f"Registry of {p} is {PYPI}, no need to change.")
                return 0
        else:
            text = replace_registry(text)
            if verbose:
                print(current_registry, "-->", PYPI)
    else:
        # TODO: ask each one to confirm replace
        text = replace_registry(text)
        if verbose:
            for current_registry in sorted(registry_urls):
                print(current_registry, "-->", PYPI)
    if len(download_hosts) == 1:
        current_host = download_hosts.pop()
        if current_host != HOST:
            text = replace_host(text)
            if verbose:
                print(current_host, "-->", HOST)
    elif download_hosts:
        # TODO: ask each one to confirm replace
        text = replace_host(text)
        if verbose:
            for current_host in sorted(download_hosts):
                print(current_host, "-->", HOST)
    size = p.write_text(text, encoding="utf-8")
    if verbose:
        print(f"Updated {p} with {size} bytes.")
    return 1


if __name__ == "__main__":
    sys.exit(main())
```

---

_Comment by @dariocurr on 2025-07-23 14:57_

this is still the reason why we cannot use `uv` in our CI

---

_Comment by @mttbernardini on 2025-08-22 10:06_

Accessing PyPI from China is either slow or impossible because of the GFW.

The usual workflow here is to use a PyPI mirror like the one provided by [Tsinghua University](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/). However, for my colleagues in UK, using the mirror makes no sense since they can just access PyPI directly, so enforcing it in the `uv.lock` is not practical.

Tools like `pdm` (or `pip`) can be instructed to use an alternative default index via its user-config `~/.config/pdm/config.toml`, and since `pdm.lock` doesn't include wheel urls (by default), I can get the benefit of a `pdm sync` using the Tsinghua mirror instead of PyPI, speeding up my workflow.

I would like to see `uv` also respecting the configuration in `~/.config/uv/uv.toml` for the default index, the very page I linked from Tsingua University suggests this:

```toml
[[index]]
url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple/"
default = true
```

But since `uv` honours the urls in the `uv.lock`, the config above is ignored (the config above is only respected for `uv tool`/`uvx` I believe), and my UX with `uv sync` becomes unbearable.

I'm for either option in:
1. Don't store the urls in the `uv.lock` and always recompute them based on the relevant `uv.toml` and `pyproject.toml` configs.
2. Store the canonical urls in the `uv.lock`, but add a proxying mechanism so that if the `default` index is re-defined (i.e. via user-config `uv.toml`), then the `uv.lock` urls for the default index are ignored and the proxy index is queried instead.

Either approach has a "hermeticity" issue, but the second one moves it to the proxy scenario, leaving the normal scenario to be efficient/hermetic, assuming that proxying may be less common than using default behaviors.

---

_Unassigned @jtfmumm by @konstin on 2025-08-22 10:07_

---

_Comment by @jamesharris-garmin on 2025-08-28 13:31_

> Perhaps we could do an informal poll using emoji reactions on this comment:
> 
>   * **Solution 1: Omit URLs from the lockfile (The "Poetry" / "Separation of Concerns" model):** React with ❤️
> 
>   * **Solution 2: Canonical URL with a Proxy override (The current PR approach):** React with 🚀
> 
>   * **Solution 3: Full URL Rewriting / Remapping (The "Git" / "Pixi" model):** React with 👀
> 
>   * **None of the above / More discussion needed:** React with 🤔

As much as I personally prefer the git/pixi model or even the current PR, my current issue is that we are currently trying to transition our mirrors from one backend mirror provider (sonatype nexus) to another (artifactory) which use different url schemes for actually hosting the files. This means nothing short of solution 1 would survive this use case.

---

_Comment by @jamesharris-garmin on 2025-08-29 18:22_

Just to clarify our issue: I work with a private repository that was until recently backed wholy by sonatype nexus. I.e. the domain name on the internal network used a round robin load balancer to route calls to the service to one of 8 different nodes running sonatype nexus. As part of a gradual scaling cutover, we are trying to move the backend server for this to artifactory. So our pool in the load balancer currently is 6 nexus nodes and 2 artifactory nodes.

Since there is no standard format for the direct URLs to the wheels/sdists Sonatype and Jfrog chose different internal schemes to provide their packages. This means that any time we resolve that domain name, we have a 1 in 4 chance of resolving to a server that is hosting the file we want in a completely different URL.

Would a possible solution in the installer be to do the following?

```
1. Attempt to download the package directly from the stored url.
2. If the download fails to complete, ask the registry where the archive is stored and fetch the archive from there?
```

This would potentially slow down the initial sync on a system using a mirror, but the stored hash of the artifacts would verify that the artifacts are correct.

An alternative approach would be to add a flag to to the `uv lock` command that would allow you to refresh the urls while keeping all other attributes of the locked data the same.

It seems like, lacking a standard URL for sourced packages, this level of hermesticity is actively hostile to the goal of supporting widely available mirroring configurations, or even the ability to provide lock files as part of publicly distributable projects.

---

_Comment by @jamesharris-garmin on 2025-08-29 18:25_

The only work around we have been able to use to handle this is, instead of using `uv sync` to setup the project, we use (in bash)

```bash
uv venv
uv pip install -r <(uv export --format requirements.txt --frozen)
```
This will not be as clean or thurough as a uv_sync because it won't remove dependencies from the environment. but it works well enough for CI environments where the goal is to populate a newly created build environment with the installed requirements.

---

_Comment by @notatallshaw on 2025-08-29 18:59_

> uv venv
> uv pip install -r <(uv export --format requirements.txt --frozen)
> 
> This will not be as clean or thurough as a uv_sync because it won't remove dependencies from the environment.

Are you aware of `uv pip sync`? https://docs.astral.sh/uv/reference/cli/#uv-pip-sync


---

_Comment by @Wim-De-Clercq on 2025-09-02 10:25_

I like to add that when using a locally cached wheelhouse directory instead of index urls and 
```
export UV_FIND_LINKS=~/.cache/pip-wheelhouse
``` 
as a variable. You end up with with local paths in the lockfile as well. Similar to the index urls.

```
source = { registry = "../../.cache/pip-wheelhouse" }
```
I think this makes even less sense than urls in the lock file. This requires my colleagues and servers to use the same folder structure, not only for the cache folder. But also it is relative to where the project is located in relation to the cache folder.
A local file path should not end up in a lock file.

---

_Comment by @jamesharris-garmin on 2025-09-03 13:31_

> > uv venv
> > uv pip install -r <(uv export --format requirements.txt --frozen)
> > This will not be as clean or thurough as a uv_sync because it won't remove dependencies from the environment.
> 
> Are you aware of `uv pip sync`? https://docs.astral.sh/uv/reference/cli/#uv-pip-sync

I was not aware of this and this basically covers my workaround case. Thanks @notatallshaw!

 But again. This needs to be configurable or the hermesticity vs. reproducibility problem will break the ability to share lockfiles and properly mirror pypi with all available public solutions.

---

_Comment by @powercoconola on 2025-09-09 21:35_

> this is still the reason why we cannot use `uv` in our CI

Agreed, hoping this is solved soon. No one can use my projects I build due to my custom `UV_INDEX` littered in the lockfiles and `pyproject.toml`. I am having to go back and update this.

I initially wanted to set `UV_INDEX` for use with `uv tool install` only, but did not know it also affected `uv lock`, etc. Maybe we can have some separation and have a `UV_TOOL_INDEX`?

---

_Comment by @lorenzosciarra on 2025-09-10 18:12_

I also have a use case in which urls in lock file prevent me from switching from PDM to UV (actually there are a couple considering that the 'uv sync -U' doesn't yet update the pyproject.toml pinned versions, but this is another story):

In my project we use different AWS accounts for different environments (Test, Staging, Prod), and we publish shared Python libraries to the corresponding CodeArtifact repositories. Finally we use them as dependencies of other modules. This is just fine with PDM because index urls and credentials can be entirely managed in the pdm.toml, which is not pushed to git, while the pdm.lock file is agnostic and doesn't change. It can't be achieved with UV at the moment instead, because syncing without locking will try to install libraries from the wrong environment, and locking will cause the lock file to continuously change.

---

_Comment by @jamesharris-garmin on 2025-09-11 16:16_

Did we actually have a mechanism to force re-write the urls from the versions in the lock file while preserving the versions we are interested in?

---

_Comment by @jessekv on 2025-09-17 15:20_

The big reason I switched from requirements.txt to a lockfile is that I want to use the lockfile's hashes to ensure the integrity of the environment, regardless of the provenance. Sometimes, where I am installing, I don't have an easy template-friendly URL transformation (or even a URL to transform to).






---

_Comment by @jamesharris-garmin on 2025-10-07 16:39_

this is less than ideal because it requires checking in changes but if you need to refresh the urls for packages you can run `uv lock --refresh` with your mirror settings and it will re-write the lock file's url lookups to point to the correct locations.

I am not sure if this ensures hashing.

---

_Label `wish` removed by @konstin on 2025-12-18 15:46_

---

_Label `enhancement` added by @konstin on 2025-12-18 15:46_

---

_Label `needs-design` added by @konstin on 2025-12-18 15:46_

---
