```yaml
number: 9195
title: "Publishing on Gitlab registry with `uv publish` needs extended access rights"
type: issue
state: closed
author: ThomasHezard
labels: []
assignees: []
created_at: 2024-11-18T13:45:26Z
updated_at: 2025-11-13T14:28:28Z
url: https://github.com/astral-sh/uv/issues/9195
synced_at: 2026-01-12T15:59:44Z
```

# Publishing on Gitlab registry with `uv publish` needs extended access rights

---

_@ThomasHezard_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hello,

I am very new to `uv` but I find the project very interesting. Coming from the system-level programming world (C/C++ and Cmake on massive mono-repos with hundreds of targets and tons of automation tools), `uv` is the first tool, amongst those I encountered, which can do project management in the Python world the way I like it: clean, explicit and simple 👍 

I have converted one simple Python project to `uv` and I have one issue regarding publishing.   

My project is hosted on a private Gitlab instance, and this project contains a Python package which is published on the project's package registry on Gitlab.
Before `uv`, I was building my wheel using `setuptools` and pushing it using `twine`.   
The build and publication was done by a CI job, and all `twine` needs to push on the project package registry on Gitlab is the `CI_JOB_TOKEN` provided by Gitlab.

`uv publish` fails to do so using the same token. After some experiments, it seems that `uv publish` needs a token with `api` scope to work, which means I have to create one and put in my CI/CD variables, and which is not the best in terms of security.

In the end, I kept `twine` for the publishing at the end of my job for security reasons instead of using `uv publish`.

Two questions:
- Is there something I did wrong or did not understand?
- If not, is there a reason for `uv publish` on Gitlab's registry to need a token with a wider scope than `twine`?

Thank you for your help, and thanks for this very good project 🙏 

---

_Comment by @FishAlchemist on 2024-11-18 14:34_

You are using CI_JOB_TOKEN as the access token, and twine can use CI_JOB_TOKEN, but ``uv publish`` doesn't, right?
Could you please provide more specific details? Such as the version of uv you're using, and what environment variables you've set for both twine and uv publish, including the values assigned to these variables. If any of the values are confidential, you can simply label them as "confidential name.


---

_Assigned to @konstin by @konstin on 2024-11-18 14:37_

---

_Comment by @FishAlchemist on 2024-11-18 14:41_

But if it's a UV issue, should we document how to publish packages to a GitLab private registry using GitLab CI/CD?
https://docs.astral.sh/uv/guides/integration/gitlab/

---

_Comment by @konstin on 2024-11-18 14:43_

Can you share your uv invocation (with secrets redacted)?

`uv publish` shouldn't need more permissions than `twine` by default (they are using the same endpoint). If you're using `--check-url`, in this case `uv publish` does more than `twine` and you may need a token that support the scope for the index page itself, too.

---

_Comment by @ThomasHezard on 2024-11-18 18:30_

Here is my final script using `twine`, which is working, all ENV variables are provided by Gitlab itself:
```bash
uv build
export TWINE_USERNAME="gitlab-ci-token"
export TWINE_PASSWORD=$CI_JOB_TOKEN
uv run --link-mode=copy --no-project --with twine python -m twine upload --repository-url $CI_SERVER_URL/api/v4/projects/$CI_PROJECT_ID/packages/pypi dist/*.whl
```

My `uv publish` equivalent is the following:
```bash
uv build
uv publish --publish-url $CI_SERVER_URL/api/v4/projects/$CI_PROJECT_ID/packages/pypi --token $CI_JOB_TOKEN dist/*.whl
```
which gets the following error (confidential data replaced by `XXX`):
```
error: Failed to publish `dist/XXX.whl` to XXX/api/v4/projects/XXX/packages/pypi
  Caused by: Upload failed with status code 401 Unauthorized. Server says: {"message":"401 Unauthorized"}
```

The only way I found to fix this is to replace `CI_JOB_TOKEN` with a custom CI variable containing a token with `api` scope, which is not an acceptable solution for me.

As for the environement:
- The scripts are running in the `ghcr.io/astral-sh/uv:debian` docker image (with up-to-date images, I verified all these error while writing this).
- python is set to 3.10 using a `.python-version` file

---

[EDIT] I realise just now that my `twine` method uses a custom username which is not `__token__` for the authentification (as [documented by Gitlab](https://docs.gitlab.com/ee/user/packages/pypi_repository/#authenticate-with-a-ci-job-token)). I'll try the `uv publish` with `--password` and `--username` instead of `--token`, I'll let you know.

---

_Comment by @ThomasHezard on 2024-11-18 18:43_

I found the issue.     
The `--token` option sets the username as `__token__`. However, in Gitlab, pushing to the pypi registry using `CI_JOB_TOKEN` requires to set the username to `gitlab-ci-token`, but you can also push to the pypi registry with the username `__token__` and a token with `api` scope as password, which led my to false interpretation of what was causing the error (a bit tricky though 😉 ).  

This script works just fine 👌 
```bash
uv build
uv publish --publish-url $CI_SERVER_URL/api/v4/projects/$CI_PROJECT_ID/packages/pypi --username "gitlab-ci-token" --password $CI_JOB_TOKEN dist/*.whl
```

Some documentation about this in `uv` documentation may be interesting, but this would imply following any update on this on Gitlab documentation to update the `uv` documentation accordingly.    
Or we could ask for Gitlab to integrate a guide for publishing with `uv` in their documentation, but I doubt they will do that before `uv` gets to stable version `1.*`...

Sorry for the false alarm 🙏  

---

_Closed by @ThomasHezard on 2024-11-18 18:43_

---

_Unassigned @konstin by @konstin on 2024-11-19 11:56_

---

_Comment by @konstin on 2024-11-19 11:59_

That's indeed confusing! We unfortunately haven't fully figured out what to do with tokens yet (https://discuss.python.org/t/pep-694-upload-2-0-api-for-python-package-repositories/16879/64, https://github.com/astral-sh/uv/issues/9195#issuecomment-2483840918)

---

_Comment by @ThomasA on 2025-01-22 09:28_

I was having the exact same problem and thanks to this thread, I now know how to solve it. Thanks @ThomasHezard 👍 

---

_Comment by @joukewitteveen on 2025-11-13 14:28_

Given that others may want to know as well and given that the documentation is easier to find than the current issue, can #16432 be merged/considered?

---
