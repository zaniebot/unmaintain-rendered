```yaml
number: 6592
title: "Add Prompts to use System or Managed Python versions or add \"auto\" option for python preferences"
type: issue
state: closed
author: vasudev-gm
labels:
  - question
assignees: []
created_at: 2024-08-24T22:12:46Z
updated_at: 2024-10-16T05:14:13Z
url: https://github.com/astral-sh/uv/issues/6592
synced_at: 2026-01-12T15:59:05Z
```

# Add Prompts to use System or Managed Python versions or add "auto" option for python preferences

---

_@vasudev-gm_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Well sorry for another late Feature request. Ran into an issue at work wherein the downloaded python versions through uv were not detected when passing the command:
`uv venv --python 3.8`

until I had to re-check the documentation and pass the python preference to be system or managed with command:
`uv venv --python 3.8 -p managed `

Will it be possible for uv to prompt the user which version to install or can we set it globally to prefer only managed versions? 

Thanks.

---

_Renamed from "Add Prompts to use System or Managed Python versions or add auto option for python preference" to "Add Prompts to use System or Managed Python versions or add "auto" option for python preferences" by @vasudev-gm on 2024-08-24 22:13_

---

_Comment by @charliermarsh on 2024-08-25 02:30_

Just confirming, is this on `v0.3.0` or later?

---

_Comment by @vasudev-gm on 2024-08-25 05:44_

Yes 0.3.0 and later. I am using uv 0.3.2.


________________________________
From: Charlie Marsh ***@***.***>
Sent: Sunday, August 25, 2024 8:01:09 AM
To: astral-sh/uv ***@***.***>
Cc: Vasudev ***@***.***>; Author ***@***.***>
Subject: Re: [astral-sh/uv] Add Prompts to use System or Managed Python versions or add "auto" option for python preferences (Issue #6592)


Just confirming, is this on v0.3.0 or later?

—
Reply to this email directly, view it on GitHub<https://github.com/astral-sh/uv/issues/6592#issuecomment-2308627528>, or unsubscribe<https://github.com/notifications/unsubscribe-auth/ANCXKEYSLMM6VLF6JG7MI5DZTE6W3AVCNFSM6AAAAABNB6AI76VHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDGMBYGYZDONJSHA>.
You are receiving this because you authored the thread.Message ID: ***@***.***>


---

_Assigned to @zanieb by @charliermarsh on 2024-08-26 14:36_

---

_Comment by @zanieb on 2024-08-26 16:46_

Are you saying that you had not run `uv python install 3.8` but you wanted us to install and use a managed Python version?

`uv venv --python 3.8 -p managed ` should be the same as `uv venv --python 3.8` — the default preference is `managed`.

For global configuration, you'll want to to set [`python-preference`](https://docs.astral.sh/uv/reference/settings/#python-preference) to `"only-managed"` in a [user-level `uv.toml`](https://docs.astral.sh/uv/configuration/files/).

---

_Label `question` added by @zanieb on 2024-08-26 16:46_

---

_Comment by @vasudev-gm on 2024-08-26 16:51_

I did install the python versions. Currently I have pipenv installed and managing venv.
So there's no way to declare globally besides using toml config? I had to declare --python- preference managed in cmdline



________________________________
From: Zanie Blue ***@***.***>
Sent: Monday, August 26, 2024 10:17:13 pm
To: astral-sh/uv ***@***.***>
Cc: Vasudev ***@***.***>; Author ***@***.***>
Subject: Re: [astral-sh/uv] Add Prompts to use System or Managed Python versions or add "auto" option for python preferences (Issue #6592)


Are you saying that you had not run uv python install 3.8 but you wanted us to install and use a managed Python version?

For global configuration, you'll want to to set python-preference<https://docs.astral.sh/uv/reference/settings/#python-preference> to "only-managed" in a user-level uv.toml<https://docs.astral.sh/uv/configuration/files/>.

—
Reply to this email directly, view it on GitHub<https://github.com/astral-sh/uv/issues/6592#issuecomment-2310634669>, or unsubscribe<https://github.com/notifications/unsubscribe-auth/ANCXKE4WK6KXTXNXOOGN5STZTNLY3AVCNFSM6AAAAABNB6AI76VHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDGMJQGYZTINRWHE>.
You are receiving this because you authored the thread.Message ID: ***@***.***>



---

_Comment by @zanieb on 2024-08-26 17:51_

 I still don't quite follow what you're saying, because `--python-preference managed` matches out default and should have no effect. Anyway, you can also use `UV_PYTHON_PREFERENCE` to configure this globally.

---

_Comment by @vasudev-gm on 2024-08-26 18:30_

> I still don't quite follow what you're saying, because `--python-preference managed` matches out default and should have no effect. Anyway, you can also use `UV_PYTHON_PREFERENCE` to configure this globally.

I will try to be clear as possible. So I wanted to ask if uv options can be set globally without needing .toml file OR uv should prompt the user asking whether to use system or managed installed when python runtimes are detected without the need for passing --python-preference [managed|system] in the command line.

---

_Comment by @zanieb on 2024-08-26 18:37_

I don't think we'll block commands for interactive prompting on which Python interpreter to use. We use whatever interpreter we can find to satisfy the Python version — this default is very intentional. If you don't want to use the system interpreters at all, we have several ways to configure that globally and per invocation.

---

_Comment by @vasudev-gm on 2024-08-27 15:49_

I'll try the uv commands. Is there a documentation link for global commands for uv.


________________________________
From: Zanie Blue ***@***.***>
Sent: Tuesday, August 27, 2024 12:08:12 AM
To: astral-sh/uv ***@***.***>
Cc: Vasudev ***@***.***>; Author ***@***.***>
Subject: Re: [astral-sh/uv] Add Prompts to use System or Managed Python versions or add "auto" option for python preferences (Issue #6592)


I don't think we'll block commands for interactive prompting on which Python interpreter to use. We use whatever interpreter we can find to satisfy the Python version — this default is very intentional. If you don't want to use the system interpreters at all, we have several ways to configure that globally and per invocation.

—
Reply to this email directly, view it on GitHub<https://github.com/astral-sh/uv/issues/6592#issuecomment-2310830097>, or unsubscribe<https://github.com/notifications/unsubscribe-auth/ANCXKE4YTPMTWLEGHVOMPOLZTNYZJAVCNFSM6AAAAABNB6AI76VHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDGMJQHAZTAMBZG4>.
You are receiving this because you authored the thread.Message ID: ***@***.***>


---

_Comment by @zanieb on 2024-08-27 15:51_

Like https://docs.astral.sh/uv/reference/cli/ ?

---

_Comment by @vasudev-gm on 2024-08-27 15:53_

Didn't know there it's a web version of entire commands. Been using uv --help to find how to use the commands. Thanks


________________________________
From: Zanie Blue ***@***.***>
Sent: Tuesday, August 27, 2024 9:22:10 PM
To: astral-sh/uv ***@***.***>
Cc: Vasudev ***@***.***>; Author ***@***.***>
Subject: Re: [astral-sh/uv] Add Prompts to use System or Managed Python versions or add "auto" option for python preferences (Issue #6592)


Like https://docs.astral.sh/uv/reference/cli/ ?

—
Reply to this email directly, view it on GitHub<https://github.com/astral-sh/uv/issues/6592#issuecomment-2312937145>, or unsubscribe<https://github.com/notifications/unsubscribe-auth/ANCXKE7JGCNM3DXWTZNWQBTZTSOCVAVCNFSM6AAAAABNB6AI76VHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDGMJSHEZTOMJUGU>.
You are receiving this because you authored the thread.Message ID: ***@***.***>


---

_Comment by @zanieb on 2024-08-27 16:05_

Yep no problem! It's generated from the `uv help` reference which is more verbose than `uv --help`.

---

_Closed by @vasudev-gm on 2024-10-16 05:14_

---
