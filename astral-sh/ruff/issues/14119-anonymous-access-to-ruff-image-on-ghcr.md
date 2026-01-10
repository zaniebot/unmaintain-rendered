```yaml
number: 14119
title: Anonymous access to ruff image on ghcr
type: issue
state: closed
author: mjbear
labels:
  - question
assignees: []
created_at: 2024-11-06T00:56:24Z
updated_at: 2024-11-07T13:23:16Z
url: https://github.com/astral-sh/ruff/issues/14119
synced_at: 2026-01-10T11:09:55Z
```

# Anonymous access to ruff image on ghcr

---

_Issue opened by @mjbear on 2024-11-06 00:56_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Hello!

Thank you for the [official `ruff` Docker image](https://github.com/astral-sh/ruff/issues/5072#issuecomment-1845495405) on ghcr.io!

As it stands currently even though the image is public, one has to authenticate to ghcr.io first before being able to pull the image. None of this authentication appears to be necessary for the official Docker registry.

```bash
# login required, otherwise access is denied
docker login ghcr.io

docker run --rm -v ${PWD}:/workdir ghcr.io/astral-sh/ruff:0.7.2 check /workdir
```

I tried to monkey with a repo of mine to see if I could get `docker run` to pull a package without getting access denied. The items I tried did not appear to make a difference.
* https://docs.github.com/en/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#about-setting-visibility-and-access-permissions-for-packages
* https://docs.github.com/en/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#disabling-automatic-inheritance-of-access-permissions-in-an-organization

Does it make sense to publish that same image to the Docker registry?

That or run an [image](https://hub.docker.com/r/pipelinecomponents/ruff) that isn't built and published by astral.

If nothing else I might learn something about GitHub permissions from this thread. :neutral_face:

Thank you! :crab: :sparkles:

---

_Renamed from "Anonymous access to ruff container on ghcr" to "Anonymous access to ruff image on ghcr" by @mjbear on 2024-11-06 03:09_

---

_Comment by @dhruvmanila on 2024-11-06 03:18_

Interesting, I don't think login should be required. I'm able to run the same command as you've provided without login in to Docker on a M3 Pro. Can you say more about the error that you're getting?

---

_Comment by @mjbear on 2024-11-06 04:15_

> Interesting, I don't think login should be required. I'm able to run the same command as you've provided without login in to Docker on a M3 Pro. Can you say more about the error that you're getting?

Hello @dhruvmanila

Certainly. In response to a docker pull:
`docker: Error response from daemon: Head "https://ghcr.io/v2/astral-sh/ruff/manifests/0.7.2": denied: denied.`

So fun thing is, I had that output in my scroll-back buffer from earlier.
I ran `docker logout` to hopefully throw out the login secrets and get a clean slate ... no error with the `docker pull`.
New shell session ... no error for docker pull.
I was ready to break from dev for the night so I chose to reboot ('cause why not for troubleshooting) ... no error for docker pull.

I know for a fact I tried several times and got a denied error.
I got the same sort of error with my own repo I was testing against.

:shrug: I'll come back to this tomorrow or the next day to see if I can replicate the behavior. :neutral_face:

---

_Comment by @dhruvmanila on 2024-11-06 04:43_

> 🤷 I'll come back to this tomorrow or the next day to see if I can replicate the behavior. 😐

For sure, I'll leave this open for now. Feel free to come back and close this if it's resolved.

---

_Label `question` added by @dhruvmanila on 2024-11-06 04:44_

---

_Comment by @mjbear on 2024-11-07 12:56_

> > 🤷 I'll come back to this tomorrow or the next day to see if I can replicate the behavior. 😐
> 
> For sure, I'll leave this open for now. Feel free to come back and close this if it's resolved.

@dhruvmanila
Thank you for taking a peek at this.
It bothers me that it magically resolved ... though someday I'll probably encounter it again. If and when that happens I'll have to dig in and take even more detailed notes.

It's possible that there was some issue on my local workstation.
It's also possible there was something going on with the GH container registry or general GH authentication.

What I can say for certain is that I've been able to pull the astral ruff image from ghcr.io as well as an image from a repo of mine. I doubt there's docker credential caching on my local workstation ... the file has no substantial content. :laughing:

```json
{
	"auths": {}
}
```

---

_Closed by @mjbear on 2024-11-07 12:56_

---

_Comment by @dhruvmanila on 2024-11-07 13:23_

Perfect! Thanks for following up.

---
