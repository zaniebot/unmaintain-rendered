---
number: 16662
title: better describe 403 error on console
type: issue
state: open
author: gpongelli
labels:
  - enhancement
  - error messages
assignees: []
created_at: 2025-11-10T09:51:05Z
updated_at: 2025-11-10T18:31:49Z
url: https://github.com/astral-sh/uv/issues/16662
synced_at: 2026-01-07T13:12:19-06:00
---

# better describe 403 error on console

---

_Issue opened by @gpongelli on 2025-11-10 09:51_

### Summary

Hi,
I was working with uv and a private index on JFrog Artifactory, due to permission error on JFrog I always receive a 403 reporting only the link that uv was not able to download, without a clear message of what's the issue.

Then I've tried to follow the link using the same username and password via browser and I received a clearer error message from Artifactory, that uv completely missed to report into console log:

```json
{
  "errors": [
    {
      "status": 403,
      "message": "Rejected artifact download request: User <jfrog-user> is not permitted to deploy '0f/e7/aa315e6a749d9b96c2504a1ba0ba031ba2d0517e972ce22682e3fccecb09/cssselect2-0.8.0-py3-none-any.whl' into '<artifactory-cache-repo>:0f/e7/aa315e6a749d9b96c2504a1ba0ba031ba2d0517e972ce22682e3fccecb09/cssselect2-0.8.0-py3-none-any.whl'."
    }
  ]
}
```

Is it possible to forward the message field to the console that's calling uv ?
the 403 error from that json is forwarded, but the reason does not; if it could be reported let people solve their issue faster.

thanks.

### Example

_No response_

---

_Label `enhancement` added by @gpongelli on 2025-11-10 09:51_

---

_Comment by @konstin on 2025-11-10 10:38_

What does uv currently show in the CLI? Is the output different with `-v`?

---

_Comment by @gpongelli on 2025-11-10 11:25_

no, `-v` output is the same as without it and does not report any information.

the above json was obtained after login into Artifactory using the link that uv put as output.

ps 
my uv version is `uv 0.8.15 (8473ecba1 2025-09-03)` 

---

_Comment by @konstin on 2025-11-10 11:28_

What does uv show in the console log, can you copy and share the output? I also recommend updating to the latest version, the uv error output improves consistently.

---

_Comment by @gpongelli on 2025-11-10 16:56_

just upgraded uv to `uv 0.9.8 (85c5d3228 2025-11-07)` , reverting JFrog permission to have error situation, the error log with `uv lock -v` is 
```
  x No solution found when resolving dependencies:
  `-> Because icecream was not found in the package registry and python-scripts:dev depends on icecream>=2.1.7, we can conclude that python-scripts:dev's requirements are unsatisfiable.
      And because your project requires python-scripts:dev, we can conclude that your project's requirements are unsatisfiable.
```

meanwhile the error with `uv sync -v` is :
```
  x Failed to download `ruff==0.14.4`
  |-> Failed to fetch: `https://<internal-url>/artifactory/api/pypi/<repository>/packages/packages/bc/22/e58c43e641145a2b670328fb98bc384e20679b5774258b1e540207580266/ruff-0.14.4-py3-none-win_amd64.whl`
  `-> HTTP status client error (403 Forbidden) for url
      (https://<internal-url>/artifactory/api/pypi/<repository>/packages/packages/bc/22/e58c43e641145a2b670328fb98bc384e20679b5774258b1e540207580266/ruff-0.14.4-py3-none-win_amd64.whl)
  help: `ruff` (v0.14.4) was included because `python-scripts:dev` (v0.1.0) depends on `ruff`
```

trying to access to that whl url `https://<internal-url>/artifactory/api/pypi/<repository>/packages/packages/bc/22/e58c43e641145a2b670328fb98bc384e20679b5774258b1e540207580266/ruff-0.14.4-py3-none-win_amd64.whl` with web browser gives me the following json (similar to the one in description above):

```json
{
  "errors" : [ {
    "status" : 403,
    "message" : "Rejected artifact download request: User <jfrog-user> is not permitted to deploy 'bc/22/e58c43e641145a2b670328fb98bc384e20679b5774258b1e540207580266/ruff-0.14.4-py3-none-win_amd64.whl' into '<artifactory-cache-repo>:bc/22/e58c43e641145a2b670328fb98bc384e20679b5774258b1e540207580266/ruff-0.14.4-py3-none-win_amd64.whl'."
  } ]
}
```

so having the errors message text into console could help solve this kind of error faster.


obviously setting the permission correctly in JFrog let this error disappear.

thanks.

---

_Label `error messages` added by @konstin on 2025-11-10 18:31_

---
