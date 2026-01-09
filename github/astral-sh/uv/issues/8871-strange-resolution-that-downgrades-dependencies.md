---
number: 8871
title: Strange resolution that downgrades dependencies to conflicing ones
type: issue
state: closed
author: potiuk
labels: []
assignees: []
created_at: 2024-11-06T23:50:36Z
updated_at: 2024-11-07T09:55:27Z
url: https://github.com/astral-sh/uv/issues/8871
synced_at: 2026-01-07T13:12:18-06:00
---

# Strange resolution that downgrades dependencies to conflicing ones

---

_Issue opened by @potiuk on 2024-11-06 23:50_

We had a very strange issue today in Airlfow. We are using `uv` to automatically upgrade our dependencies in `canary` builds regularly and we are using `--resolution highest` with complete set of airflow dependencies (`devel-ci` extra).  Then we are running `pip check` to verify if the resulting dependencies are good. And we are using `pip check` (not `uv pip` - just plain `pip`) to double-check if everything is right.

Today we had (first time for many months!) first case where the dependencies resolved by `uv` in this way produced a `pip check` conflicting set of dependencies.

This was done with `71f9d2b` commit of `https://github.com/apache/airflow` repo - and it might change soon dependinng on the released dependencies but this was reproducible at 6 Nov 2024 11pm GMT (in case you have way to get state of the `pip` repo at a given moment in time.

The way how to reproduce it is to run this is airflow main repo (and it was done using an  earlier installed airflow (python 3.9) taken from one of the previous installations

```
uv pip install --upgrade --resolution highest --editable '.[devel-ci]' --editable ./providers --editable ./task_sdk
```

This resulted in:

```
0.603 + uv pip install --python /usr/local/bin/python --upgrade --resolution highest --editable '.[devel-ci]' --editable ./providers --editable ./task_sdk
0.643 Using Python 3.9.20 environment at /usr/local
75.59 Resolved 650 packages in 1m 14s
85.70 Prepared 41 packages in 10.09s
86.43 Uninstalled 38 packages in 728ms
86.50 Installed 41 packages in 71ms
86.50  - alembic==1.13.3
86.50  + alembic==1.14.0
86.50  + apache-airflow==3.0.0.dev0 (from file:///opt/airflow)
86.50  + apache-airflow-task-sdk==0.1.0.dev0 (from file:///opt/airflow/task_sdk)
86.50  - apispec==6.7.0
86.50  + apispec==6.7.1
86.50  - cfn-lint==1.18.3
86.50  + cfn-lint==1.18.4
86.50  - diagrams==0.23.4
86.50  + diagrams==0.24.0
86.50  - duckdb==1.1.2
86.50  + duckdb==1.1.3
86.50  - google-auth==2.35.0
86.50  + google-auth==2.36.0
86.50  - jsonpickle==3.3.0
86.50  + jsonpickle==3.4.2
86.50  - kgb==7.1.1
86.50  + kgb==7.2
86.50  + local-providers==0.1.0 (from file:///opt/airflow/providers)
86.50  - marshmallow==3.23.0
86.50  + marshmallow==3.23.1
86.50  - moto==5.0.18
86.50  + moto==5.0.20
86.50  - neo4j==5.25.0
86.50  + neo4j==5.26.0
86.50  - openai==1.53.0
86.50  + openai==1.54.3
86.50  - openlineage-integration-common==1.23.0
86.50  + openlineage-integration-common==1.24.2
86.50  - openlineage-python==1.23.0
86.50  + openlineage-python==1.24.2
86.50  - openlineage-sql==1.23.0
86.50  + openlineage-sql==1.24.2
86.50  - opentelemetry-api==1.27.0
86.50  + opentelemetry-api==1.28.0
86.50  - opentelemetry-exporter-otlp==1.27.0
86.50  + opentelemetry-exporter-otlp==1.15.0
86.50  - opentelemetry-exporter-otlp-proto-grpc==1.27.0
86.50  + opentelemetry-exporter-otlp-proto-grpc==1.15.0
86.50  - opentelemetry-exporter-otlp-proto-http==1.27.0
86.50  + opentelemetry-exporter-otlp-proto-http==1.15.0
86.50  - opentelemetry-exporter-prometheus==0.48b0
86.50  + opentelemetry-exporter-prometheus==0.49b0
86.50  - opentelemetry-proto==1.27.0
86.50  + opentelemetry-proto==1.15.0
86.50  - opentelemetry-sdk==1.27.0
86.50  + opentelemetry-sdk==1.28.0
86.50  - opentelemetry-semantic-conventions==0.48b0
86.50  + opentelemetry-semantic-conventions==0.49b0
86.50  - oracledb==2.4.1
86.50  + oracledb==2.5.0
86.50  - orjson==3.10.10
86.50  + orjson==3.10.11
86.50  - pygithub==2.4.0
86.50  + pygithub==2.5.0
86.50  - python-telegram-bot==21.6
86.50  + python-telegram-bot==21.7
86.50  - regex==2024.9.11
86.50  + regex==2024.11.6
86.50  - rich==13.9.3
86.50  + rich==13.9.4
86.50  - rich-argparse==1.5.2
86.50  + rich-argparse==1.6.0
86.50  - rpds-py==0.20.1
86.50  + rpds-py==0.21.0
86.50  - ruff==0.7.1
86.50  + ruff==0.7.2
86.50  - sentry-sdk==2.17.0
86.50  + sentry-sdk==2.18.0
86.50  - tqdm==4.66.6
86.50  + tqdm==4.67.0
86.50  - types-pymysql==1.1.0.20240524
86.50  + types-pymysql==1.1.0.20241103
86.50  - types-setuptools==75.2.0.20241025
86.50  + types-setuptools==75.3.0.20241105
86.50  - yandexcloud==0.323.0
86.50  + yandexcloud==0.324.0
86.50  - ydb==3.18.6
86.50  + ydb==3.18.8
```

And then the following  `pip check` reported that there are conflicting dependencies:

```
86.86 Checking packaging tools for system Python installation: /usr/local/bin/python
86.86
86.92
86.92 Running 'pip check'
86.92
89.11 opentelemetry-exporter-otlp-proto-common 1.27.0 has requirement opentelemetry-proto==1.27.0, but you have opentelemetry-proto 1.15.0.
```

Apparently `uv` resolving downgraded a number of opentelemetry-exporter dependencies from 1.27.0 to 1.15.0 for no apparent reason.

Whe I repeated the same but adding `opentelemetry-proto>=1.27.0` to the `uv pip install` command, the resolution resolved all those opentelemetry-exporter dependencies to 1.27.0 and `pip check` stopped complaining.

Pretty weird and migh be difficult to fully reproduce from a clean state, but this was reproducible and failed other's development environments today as well. I workaround it for now by adding the `opentelemetry-proto>=1.27.0` to our eager-upgrade command but maybe from the description it will be pretty obvious for you what could have happened.

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @potiuk on 2024-11-06 23:50_

cc: @notatallshaw -> maybe you also can make a guess here ?

---

_Referenced in [apache/airflow#43768](../../apache/airflow/pulls/43768.md) on 2024-11-06 23:57_

---

_Comment by @ferruzzi on 2024-11-07 00:01_

FWIW, can confirm I ran into this locally as well, independent of Jarek, with the same results.

---

_Comment by @charliermarsh on 2024-11-07 00:21_

Thanks! I can take a look but note that a bunch of dependencies _did_ upgrade, so it's possible that some upgrade introduced an upper-bound constraint elsewhere that led the resolver to "need" to downgrade others.

Separately, where is `opentelemetry-exporter-otlp-proto-common`? Is that installed in a prior step?

---

_Comment by @notatallshaw on 2024-11-07 00:30_

Yeah, so first thing is that two `pip install` commands are not guaranteed to produce a consistent install, a simple of example of this is if say `B` requires `A>1` and you run `pip install B` followed by `pip install A==1.0` you end up with an inconsistent environment. This is as true of the `uv pip` interface as it is of the `pip` interface.

I would reccomend to install into an existing environment where you don't want packages to regress and also install additional packages, while keeping a consistent environment, is to do something like this:

```
$ uv pip freeze | sed 's/==/>=/g' | cat requirements.txt - | uv pip compile - -o output.txt  --upgrade
$ uv pip install -r output.txt
```

There are options on `freeze` and `compile` you may need to play around with to fit your use case.

---

_Comment by @potiuk on 2024-11-07 00:43_

> Separately, where is opentelemetry-exporter-otlp-proto-common? Is that installed in a prior step?

Yes. It has been installed in one of the previous successful resolutions (that's why it is difficult to reproduce ) - because in Airflow we use the "previously successfuly tested" set of dependencies from the last "green" main build as a starting point for the upgrade. In this case the "starting" point of installed dependencies were build in this action run:

> https://github.com/apache/airflow/actions/runs/11704598329

And I think the easiest way to get the starting point is to download and run the image it has used.

This can be done with (on amd64 architecture)

```
docker pull ghcr.io/apache/airflow/main/ci/python3.9:f05716bb0b92f180a40150bef8c2cbab153dd361
docker run -it ghcr.io/apache/airflow/main/ci/python3.9:f05716bb0b92f180a40150bef8c2cbab153dd361 bash
```

That should get you inside of the `airflow` image used and then likely (I am not at intel machine right now) - running the command above should reproduce the issue:

```
uv pip install --python /usr/local/bin/python --upgrade --resolution highest --editable '.[devel-ci]' --editable ./providers --editable ./task_sdk
```


@notatallshaw 

> Yeah, so first thing is that two pip install commands are not guaranteed to produce a consistent install

Yeah. I absolutely get it but the problem is that:

a) this resolution resulted in a set of dependencies that `pip check` considered invalid (they were in fact invalid)
b) when I added `opentelemetry-proto>=1.27.0`, the opentelmetry dependencies were not downgraded AND the produced resoultion was "valid" according to the pip check.

So I don't mind to have different resolutions - but this time it caused **invalid** resolution not only different. And that is problematic.

Also, when I tried to apply workaround with `opentelemetry-proto>=1.27.0` even stranger thing happened.
https://github.com/apache/airflow/pull/43768#issuecomment-2461071604

Apparently when I tried to install airflow in non-editable mode trying to install provider packages (rather than their dependencies) build locally and `opentelemetry-proto>=1.27.0` - for some strange reason, uv attempted (and failed) to download some very, very old `apache-beam=2.0.0` which is REALLY strange as just adding `opentelemetry-proto>=1.27.0` should not have caused to attempt apache-beam to be downgraded, especially that a moment ago `apache-beam==2.60.0` was perfectly fine and resolved correctly. 








---

_Comment by @potiuk on 2024-11-07 00:48_

Also @notatallshaw 

> I would reccomend to install into an existing environment where you don't want packages to regress and also install additional packages, while keeping a consistent environment, is to do something like this:
>
> $ uv pip freeze | sed 's/==/>=/g' | cat requirements.txt - | uv pip compile - -o output.txt       
> $ uv pip install -r output.txt

This is precisely what we do not want to do. that command that we are running, is not to keep consistent environment, it is used to find the **new** best resolution taking into account all the packages that were released since last time we resolved the dependencies. Basically "find me the consistent set of dependencies for `.[devel-ci]` that `highest resolution` provides". 

This particular line of code does not care about "keeping" the environment from previous run, I am really interested in getting "new consistent environment taking into account all packages released recently". 

---

_Comment by @charliermarsh on 2024-11-07 00:51_

> this resolution resulted in a set of dependencies that pip check considered invalid (they were in fact invalid)

But isn't in invalid taking into account a package that was already installed in the virtual environment _but not part of the resolution_? The second `uv pip install` doesn't know anything about `opentelemetry-exporter-otlp-proto-common` or its constraints, right? (Neither would `pip install`.)

---

_Comment by @notatallshaw on 2024-11-07 00:51_

> So I don't mind to have different resolutions - but this time it caused **invalid** resolution not only different. And that is problematic.

That's what I'm saying, two `pip install` commands can produce an invalid resolution. It doesn't matter if you use uv or pip, there is always chance of this outcome.



> This is precisely what we do not want to do. that command that we are running, is not to keep consistent environment, it is used to find the **new** best resolution taking into account all the packages that were released since last time we resolved the dependencies.

That command I gave should give a new best resolution as well as guaranteeing being consistent.

---

_Comment by @potiuk on 2024-11-07 01:06_

> That command I gave should give a new best resolution as well as guaranteeing being consistent.

Ah . I see - interesting - I did not notice the sed command replacing `==` with `>=`.... But this is not exactly what I want. Sometimes I want the packages to regress. For example it might be that a new version of a package is relased that has < limit on a version of another package I already have installed - and maybe few others as well (for example when that package has a bug discovered that caused few maintainers to limit it) - in this case (and it happened in the past) - I want the new resolution to (possibly) downgreade that package and upgrade the others.

And .... so far it worked. But for some reason it caused this invalid resolution.

> That's what I'm saying, two pip install commands can produce an invalid resolution. It doesn't matter if you use uv or pip, there is always chance of this outcome.

Aha.. Yeah. Actually, that made me think... I think you are likely right and I start seeing scenario when that could have happened - probably when one of the new packages stops having another package as dependency then probably that could have caused that dependency to be "left over" by the resolution and cause the conflict. 

Thanks! that gave me a clue. I have to look a bit deeper, but I think I know the scenario and indeed it is likely not a bug in uv. Sorry @charliermarsh -> I will take a deeper look and report back here later (it's after midnight for me)

---

_Comment by @notatallshaw on 2024-11-07 01:17_

> Ah . I see - interesting - I did not notice the sed command replacing `==` with `>=`.... But this is not exactly what I want. Sometimes I want the packages to regress.

Well you could do:

```
$ uv pip freeze | cut -d'=' -f1 | cat requirements.txt - | uv pip compile - -o output.txt  --upgrade
$ uv pip install -r output.txt
```

> And .... so far it worked. But for some reason it caused this invalid resolution.

I really think it's just about luck, most the time it will work fine, and then a new dependency is created and it just happens the resolution hits something surprising and becomes invalid.

> Aha.. Yeah. Actually, that made me think... I think you are likely right and I start seeing scenario when that could have happened - probably when one of the new packages stops having another package as dependency then probably that could have caused that dependency to be "left over" by the resolution and cause the conflict.

Yes, similiar to my A / B example, this will happen with a transative dependencies from the first install, and then likely when it becomes a new dependency or transative dependency in the second install.

---

_Comment by @potiuk on 2024-11-07 01:17_

Oh yeah. 

I do not know *why* opentelemetry has been downgraded (though it is secondary), but I know how the conflict happened.

This is because caching is hard (and especially cache invalidation),

The opentelemetry-*-http and -grpc 1.27 had opentelemetry-*-common 1.27 as dependency, BUT http/gprpc 1.15 DID NOT... This means that when everything from opentelemetry has been downgraded, the common one was not even considered for 1.15 as dependency so it has not been downgraded together - it's cached version from previous resolution was simply not considered in the final reslution.

That is very interesting one. And likely I can solve it by **just** avoiding to use the cached version from last run - which I can easily do by increasing the build epoch in the dockerfile. 

Thanks. I see that it is NOT uv issue @charliermarsh, so unless you would like to handle a case that `uv` should h

---

_Comment by @potiuk on 2024-11-07 01:25_

Closing. Thanks for the discussion. It's been very helpful to get to the root of the problem.

---

_Closed by @potiuk on 2024-11-07 01:25_

---

_Comment by @notatallshaw on 2024-11-07 01:32_

Btw, taking a step back, I think you could produce sufficent constraints with just running `uv pip compile` commands, I don't think you need to base your new resolution on a previous install any more, `uv pip compile` is very powerful, especially with `--universal` options and others.

---

_Comment by @potiuk on 2024-11-07 01:57_

> Btw, taking a step back, I think you could produce sufficent constraints with just running uv pip compile commands, I don't think you need to base your new resolution on a previous install any more, uv pip compile is very powerful, especially with --universal options and others.

Not really - because we are doing it in Docker container, and we do not want to have cache of UV when we are doing it on the server. For airflow, UV cache for all our dependencies increases the size of our image by 2GB (~ 100%) - so we really do not want to keep cache there. While `uv` resolution is way faster than `pip`, without the cache and starting from 0, such resolution will take minutes (mostly because downloading and building some packages takes time not the resolution itself). But when when we base it on installed airflow, it takes seconds, because we usually only download and install those packages that have been upgraded. Maybe we can revisit that at some point in time and do benchmarking - but just compiling and installing gssapi (which almost never change) takes more than 2 minutes, similarly krb5- by having those dependencies installed - serving effectively as both - installed packages and cache - we save precious minutes of build time in our CI.



---

_Referenced in [apache/airflow#43770](../../apache/airflow/pulls/43770.md) on 2024-11-07 02:02_

---

_Comment by @potiuk on 2024-11-07 02:08_

BTW. Confrmed locally https://github.com/apache/airflow/pull/43770 - bumping the EPOCH (i.e. cleaning the cache) solved the problem for me.

> There are 2 hard problems in computer science: cache invalidation, naming things, and off-by-1 errors.



---

_Comment by @notatallshaw on 2024-11-07 02:30_

> Not really - because we are doing it in Docker container, and we do not want to have cache of UV when we are doing it on the server. For airflow, UV cache for all our dependencies increases the size of our image by 2GB (~ 100%) - so we really do not want to keep cache there.

`uv pip compile` almost never downloads a full package, even when it resolves sdists, due to assumptions uv makes when resolving.

> While uv resolution is way faster than pip, without the cache and starting from 0, such resolution will take minutes (mostly because downloading and building some packages takes time not the resolution itself). 

I think you'll find even with `--no-cache` that `uv pip compile` takes seconds, even with `--universal` which will resolve every version of Python simultaneously.

---

_Comment by @zanieb on 2024-11-07 03:10_

I don't know the details of your Docker setup, but in case you don't know, you could use a [cache mount](https://docs.astral.sh/uv/guides/integration/docker/#caching)

---

_Comment by @potiuk on 2024-11-07 09:55_

> I don't know the details of your Docker setup, but in case you don't know, you could use a [cache mount](https://docs.astral.sh/uv/guides/integration/docker/#caching)

We can't unfortunately, because this is docker container built on Github Runner in CI - so we have no cache mount available between builds. We could potentially use cache of github Runners to store the cached volume mount - but this cache in itself is limited to 10GB for everything and we would run out of it very quickly with different branches we have.

We are planning to explore using artifacts for this cache - that woudl also help us with simplifying our workflows - https://github.com/apache/airflow/issues/43268 - but this will be pretty complex to implement and so far i found that using installed packaged from the "branch tip" of airflow (with disabled PIP or UV caching) and storing this as a single layer in our image with incrementally adding new layer with new packages, provides the best balance between the image size and time of buiilds.

Note also that those images are pulled locally when you want to replicate CI development environment - because building those image takes also at least 5-10 minutes we optimize that by using github registry cache when we rebuild the images locally - which saves up to 5-8 minutes every time you build such image. 

So it's not **that** straightforward. But there is a way to simplify it in the future and maybe indeed use a bit more of the cache that UV provides.

---
