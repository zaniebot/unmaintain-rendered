```yaml
number: 9920
title: "Enable user-level management of index credentials via uv (& keyring)"
type: pull_request
state: open
author: stoney95
labels: []
assignees: []
base: main
head: main
created_at: 2024-12-15T20:10:54Z
updated_at: 2025-04-14T08:00:32Z
url: https://github.com/astral-sh/uv/pull/9920
synced_at: 2026-01-10T11:10:34Z
```

# Enable user-level management of index credentials via uv (& keyring)

---

_Pull request opened by @stoney95 on 2024-12-15 20:10_

## Summary
Currently reading credentials from `keyring` is only supported when a username is provided in the URL of the index. This prohibits to define indexes - in pyproject.toml - that are shared within a team. See these two issues for further details:
- https://github.com/astral-sh/uv/issues/8810
- https://github.com/astral-sh/uv/issues/9165

In general this PR provides two things:

### Setting credentials for an index via CLI
With this MR you can run `uv index credentials add --name <name-of-the-index> [--username <username>]`
This will ask for the password of the user. A keyring entry will be made with the url of the index, the username and the password. The username and the index will be appended to "<uv_cache_dir>/auth.toml". "auth.toml" has the following structure

```toml
[indexes.index1]
username = "my_user1"

[indexes.index2]
username = "my_other_user"
```

### Using credentials 
When reading the credentials in `uv_distribution_types::index::Index.credentials` it's now additionally checked if credentials have been configured via "<uv_cache_dir>/auth.toml" and `keyring`. The current implementation of reading the credentials from the environment variables `UV_INDEX_XXX` has priority over `keyring` authentication. 

## Test Plan
I have added unit tests to the newly defined `uv_auth::keyring_config` which takes care of loading, storing and modifying the auth configuration. 

I also want to add a test for `uv_auth::credentials::Credentials.from_keyring`. But am currently struggling with mocking keyring and config file.

I manually tested the new command `uv index credentials add --name <name-of-the-index>` as I could not find examples for testing commands.


## Further remarks
### I am a noobie in this context
- I am new to rust
- I am new to uv

=> I am curious about your opinion and suggestions for improvements

### Not completely ready
The current version of the PR is a draft. Especially in regards of error handling and logging. Also testing can be improved. 
I open this PR to discuss the direction in which the implementation is heading. As I am new to rust & uv I would also like to receive your guideance upfront 🙂 


---

_Converted to draft by @stoney95 on 2024-12-16 07:32_

---

_Comment by @bschoenmaeckers on 2024-12-16 13:19_

It would be nice to add a `--password <password>` option and store/retrieve it using the keyring crate. It should still fall back to the subprocess method for dynamic passwords.

---

_Assigned to @zanieb by @zanieb on 2024-12-16 21:09_

---

_Comment by @stoney95 on 2024-12-16 21:31_

@bschoenmaeckers, I think adding `--password <password>` can be seen independently of the keyring crate (https://docs.rs/keyring/latest/keyring/?). 

One part is implementing an `import` keyring-provider. This would rely on the keyring crate. Also, it should be implemented independently of this topic, if it is required.

The other part is providing a `--password` option. Which could be processed with any implemented keyring-provider - currently only `subprocess`. I can add this part to this PR. 

---

_Comment by @farndt on 2024-12-17 08:09_

@stoney95 Thx for starting integration of kering-rs into uv!

How to use credentials already defined in credential store?

---

_Comment by @stoney95 on 2024-12-30 14:04_

@zanieb, clippy is configured to not allow the usage of `fs::File`, `fs::read_to_string` & `fs::remove_file` (see [this check]( https://github.com/astral-sh/uv/actions/runs/12546987083/job/34983707446?pr=9920)). Should the async version provided by `tokio` be used instead? Or is there another reason why the usage is not allowed?

---

_Comment by @lejmr on 2025-01-02 22:03_

I was super curious whether it would be difficult to interact with keyring-rs directly, so I quickly implemented alternative Credential constructor (tests are necessary!), but it works with locally configured auth.toml and hand-crafted secret in keychain already..

I like what you tried to achieve, but I think it would be good to implement the configuration to be similar to poetry, so users don't have to think and be surprised.

Have a look here: https://github.com/astral-sh/uv/compare/main...lejmr:uv:simple-poetry-like-version?expand=1

---

_Comment by @stoney95 on 2025-01-12 14:48_

@lejmr, thanks for your suggestion. To clarify, I see two suggestions made from your end. 

1. How can we make use of the `keyring` crate.
2. Let's discuss the naming of the index command. 

Do you agree with this summary? 

## Personal opinion

### `keyring` crate
As mentioned before, I think the discussion of using the keyring crate is independent of this topic. As there is a global keyring provider (see the [uv docs](https://docs.astral.sh/uv/configuration/authentication/#http-authentication), and [code section](https://github.com/astral-sh/uv/blob/main/crates/uv-auth/src/keyring.rs)), it should be controlled via this if the keyring crate (use `--keyring-provider import`) or a subprocess calling keyring (use `--keyring-provider subprocess`) is used. 

Personally I don't understand what's the benefit of leaving the choice to the user. Also, I would go for using the `keyring` crate. But as the global keyring provider is currently in place, we should not just ignore it.
I would be very glad, if somebody could explain, why the choice of the keyring provider is left to the user.

### Index command naming
With the current implementation the credentials could be set with `uv index credentials add --name <name-of-the-index>`. 

You suggest to follow the wording of poetry to make it easier for users to adopt, e.g. `uv config http-basic.<name-of-the-index> <username> <password>`
For reference see the [poetry docs](https://python-poetry.org/docs/repositories/#installing-from-private-package-sources).

Personally I find the wording of poetry misleading, but am curious about other opinions.

---

_Comment by @zanieb on 2025-01-12 15:06_

Hey @stoney95 sorry I missed your ping!

Reviewing this is on my queue but I'm working through that holiday backlog still.

Regarding

> @zanieb, clippy is configured to not allow the usage of fs::File, fs::read_to_string & fs::remove_file (see [this check](https://github.com/astral-sh/uv/actions/runs/12546987083/job/34983707446?pr=9920)).

We use `fs_err` instead where we can, because it includes additional context in error messages.

---

_Comment by @lejmr on 2025-01-12 23:14_

@stoney95, let me write my grains of salt..

1) Naming.. I don't have strong opinion about uv command structure - `uv index credentials` make sense to me. What is important to understand, is that `uv` is lacking ability to define username/password out of the pyproject.toml or lock files, and this functionality is critical for team cooperation!  In other words, `uv auth` would be good for me too. Basically, the core of this functionality  should be "user interface for managing usernames per index", and password management is just a bonus. _(no strong naming opinion here!)_

Add/Rm etc is bad naming to me because what really happens is upsert, so add is confusing.


2) crate from keyring. Well, I primarily wanted to test what is possible. I think using subprocess is better since the official keyring has support for "3rd party backends" hence it is more flexible. Using the keyring-rs should be implemented as `native` flavor to Keyprovide in my opinion, but having proper `subprocess` can be quite sufficient!

Regards 2), I think your implementation is just more complicated than what is necessary. I took shortcut by implementing `Credentials::from_keyring`  function and hacking it into `Index::credentials` function. What I think is proper:

- `Credentials::from_request` can load username from `auth.toml`
  -  It can be done simply following this logic: https://github.com/pypa/pip/blob/ae5fff36b0aad6e5e0037884927eaa29163c0611/src/pip/_internal/network/auth.py#L282
  - Convert request URL to IndexURL in order to identify which index should be used (Sadly fn handle  is a few level deeper from where Index was known.. so converting Request.url to Index sounds like waste of time)
- `AuthMiddleware::handle` loads credentials on the first line, so if we return credentials with just `username` we can then let rest flow ... keyring will look up for given URL&username



---

_Comment by @stoney95 on 2025-01-13 08:33_

@lejmr, thanks for explaining your suggestion in more details :) 

### Naming
I would opt for `uv index credentials {set|unset|list}`. This opens up the possibility to add further index commands in the future, e.g. `uv index set` which can then guide a user to configure an index. This could be nice to have. 

### Keyring crate / Complicated implementation

I am using `Credentials::from_keyring` in [`Index::credentials`](https://github.com/stoney95/uv/blob/main/crates/uv-distribution-types/src/index.rs#L158-L176). Before my adjustments this would first check if credentials are present in env variables. As a second option it will check if it can retrieve credentials from the url. 

If I understand your suggestion correctly we could modify [`Credentials::from_url`](https://github.com/stoney95/uv/blob/main/crates/uv-auth/src/credentials.rs#L127-L150) instead of trying to load credentials from keyring directly. 

So, we could make `Credentials::from_keyring` private and call it from within [`Credentials::from_url` when no username and password are encoded in the url](https://github.com/stoney95/uv/blob/main/crates/uv-auth/src/credentials.rs#L128-L130). What's your opinion on that? (@zanieb, I would also be interested to hear your opinion on that)

---

_Comment by @lejmr on 2025-01-13 09:04_

Keyring) if we modify ::from_url, so it loads username from auth.toml for the given URL, we don't need the from_keyring  at all because the secret is loaded by middleware, https://github.com/stoney95/uv/blob/main/crates/uv-auth/src/middleware.rs#L203 - which is already existing code, so we won't create any duplicity.

---

_Comment by @nbaju1 on 2025-01-22 08:36_

How will this interact with the index url in the uv-receipt.toml file created when installing tools with `uv tool install <tool>`? As far as I know, this index url is used to update the tool when running `uv tool upgrade <tool>`.

Context: We frequently rotate Artifactory tokens on clients, where we also update the token on the various tools that use them (poetry, pip, npm etc). Currently, uv is a bit of a pain to update due to these uv-receipt.toml files for each tool installed. This PR will help a lot with robustly updating credentials for uv, but I hope that this will work for tools installed with uv as well.

---

_Comment by @stoney95 on 2025-01-31 23:09_

@lejmr, I moved the logic for loading the username to `credentials.from_url`. Thanks for pointing out this option! :)

@nbaju1, I can't answer if this will work for tools as well. I assume that managing tools will also rely on the middleware. So, it should work, but I think the others in this thread are better suited to answer this question. 

---

_Marked ready for review by @stoney95 on 2025-02-28 14:46_

---

_Comment by @stoney95 on 2025-02-28 14:47_

@zanieb, could you give this PR a review? :)

---

_Comment by @stoney95 on 2025-03-11 14:27_

@charliermarsh, can you take a look at this PR? Or is there anything that's still missing to give it a review?

---

_Comment by @zanieb on 2025-03-11 17:23_

We're still figuring out how credential management fits into our larger user story. Projects like this involve a significant amount of design work on our end, which usually happens before a pull request is opened. In this case, the pull request will need to wait until we do that work so we can make sure it's aligned with our designs. It's nice to have this for inspiration, but it's a big deal to add a whole new interface to uv like this and we need to approach solutions carefully and holistically as we will be responsible for maintaining it in perpetuity.

---

_Comment by @stoney95 on 2025-03-13 13:08_

Thanks for sharing your - and the uv-team's - perspective, @zanieb. Did you discuss the topic internally already and can you share what you are currently considering? 

In case I can support the design phase in some way or another, please let me know. 

---
