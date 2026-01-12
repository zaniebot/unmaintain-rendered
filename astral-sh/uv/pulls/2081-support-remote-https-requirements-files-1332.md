```yaml
number: 2081
title: "Support remote `https://` requirements files (#1332)"
type: pull_request
state: merged
author: jannisko
labels:
  - configuration
assignees: []
merged: true
base: main
head: main
created_at: 2024-02-29T14:32:36Z
updated_at: 2024-03-06T06:47:48Z
url: https://github.com/astral-sh/uv/pull/2081
synced_at: 2026-01-12T16:04:51Z
```

# Support remote `https://` requirements files (#1332)

---

_@jannisko_

## Summary

Allow using http(s) urls for constraints and requirements files handed to the CLI, by handling paths starting with `http://` or `https://` differently. This allows commands for such as: `uv pip install -c https://raw.githubusercontent.com/apache/airflow/constraints-2.8.1/constraints-3.8.txt requests`.

closes #1332

## Test Plan

Testing install using a `constraints.txt` file hosted on github in the airflow repository:
https://github.com/jannisko/uv/blob/fbdc2eba8e5e8fb4160baae495daff8fb48df13f/crates/uv/tests/pip_install.rs#L1440-L1484

## Advice Needed

- filesystem/http dispatch is implemented at a relatively low level (at `crates/uv-fs/src/lib.rs#read_to_string`). Should I change some naming here so it is obvious that the function is able to dispatch?
- I kept the CLI argument for -c and -r as a PathBuf, even though now it is technically either a path or a url. We could either keep this as is for now, or implement a new enum for this case? The enum could then handle dispatch to files/http.
- Using another abstraction layer like https://docs.rs/object_store/latest/object_store/ for the files/urls/[s3] could work as well, though I ran into a bug during testing which I couldn't debug

---

_Review requested from @charliermarsh by @charliermarsh on 2024-02-29 14:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-29 14:58_

---

_Label `configuration` added by @charliermarsh on 2024-02-29 14:58_

---

_Comment by @charliermarsh on 2024-02-29 14:59_

Thanks! I've been conflicted on whether to support this but ultimately it does seem reasonable.

> For now, only manually tested. I need advice on how to test this feature. Is a test using a permalink to a GitHub repo considered stable enough for this case or do we need a temp http server for tests?

Using a permalink to a GitHub repo is totally fine and consistent with some of our other tests.

---

_Comment by @jannisko on 2024-03-01 16:33_

Ready for review now in my eyes. I tried replacing the `PathBuf` with something more expressive, like a `LocalOrHttpPath` enum, but this turned into a mess. Paths really are a good way of handling this use case IMO. Even nested requirements/constraints files work out of the box via https, as long as they are also located in the same relative position on the server.

e.g.
/inner/requirements.txt:
```
requests
-r ../requirements.txt
``` 
/requirements.txt
```
ruff
```

```
$ cargo run -- pip install -r http://localhost:8000/inner/requirements.txt
    Finished dev [unoptimized + debuginfo] target(s) in 0.19s
     Running `target/debug/uv pip install -r 'http://localhost:8000/inner/requirements.txt'`
Resolved 6 packages in 614ms
Downloaded 1 package in 2.56s
Installed 6 packages in 30ms
 + certifi==2024.2.2
 + charset-normalizer==3.3.2
 + idna==3.6
 + requests==2.31.0
 + ruff==0.3.0
 + urllib3==2.2.1
 ```

---

_Renamed from "support remote http[s] constraint.txt files (#1332)" to "support remote http[s] constraints/requirements/overrides.txt files (#1332)" by @jannisko on 2024-03-01 16:33_

---

_Comment by @charliermarsh on 2024-03-01 18:13_

Okay cool, thank you! Acknowledging that it's now blocked on me reviewing.

---

_Comment by @potiuk on 2024-03-02 08:35_

Just to add to the weight here: This is also [Highly used feature in apache-airflow](https://airflow.apache.org/docs/apache-airflow/stable/installation/installing-from-pypi.html) - if users of airflow would like to use the recommended installation option - they cannot use it directly now with uv - they need to download the constraints first. 

This caused a small hicc-up in our CI images in Airflow (as I did not notice that remove constraints installation was not working with uv. I fix it in https://github.com/apache/airflow/pull/37845 (and generally downloading constraints to CI image is a good idea) - but users trying to do reproducible installation of Airlfow should be able to use the URL, so big cheering on that one :).

---

_@charliermarsh reviewed on 2024-03-02 22:20_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:76 on 2024-03-02 22:20_

I think I would prefer to add dedicated methods for this in `requirements-txt`, for two reasons:

1. I don't think we want to allow users to provide `pyproject.toml` files as URLs for now. I'd just prefer to limit it to requirements files since no tool supports remote `pyproject.toml` (and, in practice, we're actually supposed to _build the project_ when we're given a `pyproject.toml`, so this wouldn't work correctly in all cases).
2. Adding `reqwest` as a dep to `uv-fs` feels heavy since this crate is supposed to be lightweight.

---

_@charliermarsh reviewed on 2024-03-02 22:20_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:392 on 2024-03-02 22:20_

So the join here is fine even if `filename` is a URL?

---

_@charliermarsh reviewed on 2024-03-02 22:23_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:327 on 2024-03-02 22:23_

On the question of using `Path`: Part of me feels like we should be using `VerbatimUrl` for this (used elsewhere in this crate). `VerbatimUrl` can represent both URLs and paths, but it preserves the user-provided representation, which we leverage to ensure that we _don't_ expand environment variables (like secrets) when we write results back out to `requirements.txt`. I think we would need a `join` method on it, which would perhaps be somewhat awkward... But would you mind giving it a try, and seeing if it fits? Alternatively, we can punt it to another PR -- I would be open to merging without it.

---

_@charliermarsh reviewed on 2024-03-02 22:23_

This looks good, thanks! Some comments below, only one blocking.

---

_@charliermarsh reviewed on 2024-03-02 22:31_

Oh, one other critical point: how does this work with authentication? Does pip support reading from URLs that require authentication?

We probably need to pass through our `RegistryClient` or something similarly-constructed. That would also ensure that this behavior respects the `--offline` flag.

---

_Review comment by @jannisko on `crates/uv-fs/src/lib.rs`:76 on 2024-03-04 12:09_

Very fair point. Didn't even realize `read_to_string` is only called in `RequirementsTxt::parse` and for parsing `RequirementsSource::PyprojectToml`. So this change should be easy to isolate to `RequirementsTxt` 👍

---

_@jannisko reviewed on 2024-03-04 12:09_

---

_@jannisko reviewed on 2024-03-04 12:23_

---

_Review comment by @jannisko on `crates/requirements-txt/src/lib.rs`:392 on 2024-03-04 12:23_

Yup works really well from my testing. It is basically handled like a relative path starting with the `http:` directory. The only sketchy thing I could find for now is that it doesn't survive a round trip through `.components()`:
```PathBuf::from_iter(PathBuf::from_str("http://test/abc.txt").unwrap().components())``` = `http:/test/abc.txt` (one `/` missing).

---

_@jannisko reviewed on 2024-03-04 12:26_

---

_Review comment by @jannisko on `crates/requirements-txt/src/lib.rs`:327 on 2024-03-04 12:26_

I'd like to give it a try in a separate PR!

---

_Comment by @jannisko on 2024-03-04 14:54_

> Oh, one other critical point: how does this work with authentication? Does pip support reading from URLs that require authentication?

Pip seems to ask for interactive input when the url requires auth:
```
$ pip install -r http://localhost:8000/requirements.txt
User for localhost:8000: jannis
Password: 
Requirement already satisfied: ruff in /Users/jannis_kowalick/.pyenv/versions/3.10.9/lib/python3.10/site-packages (from -r http://localhost:8000/requirements.txt (line 1)) (0.2.1)
```

With my current implementation, uv would interpret the "no auth header" message as a package name, which seems dangeous:
```
$ uv pip install -r http://localhost:8000/requirements.txt
error: Unsupported requirement in http://localhost:8000/requirements.txt at position 0
  Caused by: URL requirement must be preceded by a package name. Add the name of the package before the URL (e.g., `package_name @ https://...`).
no auth header received
^^
```

I'll try to figure out how to mirror pips behavior, and take a look at the `RegistryClient` as well.

---

_@jannisko reviewed on 2024-03-05 13:05_

---

_Review comment by @jannisko on `crates/uv-fs/src/lib.rs`:76 on 2024-03-05 13:05_

implemented in [1ef5aac](https://github.com/astral-sh/uv/pull/2081/commits/1ef5aacfb0064022b9273a0248448d232bbbb946)

---

_Comment by @jannisko on 2024-03-05 13:08_

@charliermarsh I moved the http handling into `requirements-txt` and I'm handing the `RegistryClient` over in there now too. Let me know what you think!

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-05 23:13_

---

_@charliermarsh approved on 2024-03-06 03:58_

Thank you!

---

_Comment by @charliermarsh on 2024-03-06 04:00_

@jannisko -- I mostly made cosmetic changes, but I did make one behavioral change, which is that I removed `dialoguer`. I'd like to see if this comes up in practice for users before adding support for interactively prompting credentials -- it adds complexity, and it's also yet another dependency in our tree. If we need to restore, we'll always have it here in the Git history.


---

_Renamed from "support remote http[s] constraints/requirements/overrides.txt files (#1332)" to "Support remote `https://` requirements files (#1332)" by @charliermarsh on 2024-03-06 04:03_

---

_Merged by @charliermarsh on 2024-03-06 04:18_

---

_Closed by @charliermarsh on 2024-03-06 04:18_

---

_Comment by @jannisko on 2024-03-06 06:47_

Thanks a lot @charliermarsh for all the feedback!

---
