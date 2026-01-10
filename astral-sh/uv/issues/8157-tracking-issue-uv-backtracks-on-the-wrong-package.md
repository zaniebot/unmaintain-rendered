```yaml
number: 8157
title: "Tracking issue: uv backtracks on the wrong package"
type: issue
state: closed
author: konstin
labels:
  - resolver
assignees: []
created_at: 2024-10-13T13:03:22Z
updated_at: 2024-12-16T10:39:51Z
url: https://github.com/astral-sh/uv/issues/8157
synced_at: 2026-01-10T04:36:20Z
```

# Tracking issue: uv backtracks on the wrong package

---

_Issue opened by @konstin on 2024-10-13 13:03_

In this issue, I'm collecting cases where uv backtracks unnecessarily. Please feel free to edit and add your own cases.

## Problem layout

Usually, this happens in the following way:
* We select package A
* We select A==a
* We select package B
* We select B==b
* B==b is incompatible with A==a. We try many versions of B until we get to one of the following outcomes:
  1. If B is lacking a lower bound: We arrive at an old version of B that does not depend on A anymore, but is too old for the user
  2. If B is lacking a lower bound: We arrive at a version of B that does not build anymore.
  3. We exhausted all compatible versions of B. We reject A==a and select a version of A that is compatible with a version of B, usually B==b, solving the conflict.

Cases 1. and 2. are caused by missing lower bounds, are can detected by looking for a combination of no lower bound and incompatibility. Case 3 leads to the correct resolution but makes uv really slow.

This happens because PubGrub tries to backtrack the minimal amount: Once we locked in A==a, we will try we will try any other option with any B that gives us a solution with A==a. PubGrub by itself will never switch to discarding A==a for now and trying B==b first instead. Additionally, uv package priorities usually mean we will retry A before B again.

As solution, we should track how conflicting or bad a version is: If we rejected a lot of versions of e.g. B due to A==a, we consider B a very conflicting package and A==a a very rejecting version, and should switch them to trying B first. The PubGrub implementation allows this: We are allowed to backtrack further than we need to and keep all incompatibilities intact. We only need to ensure that we pick differently in the next round of package and version selections, to not end up in a loop.

We can apply similar things to avoid building source distributions. Source distribution builds are both prohibitively expensive and prone to fail, so we should try our best to find a solution with only wheel. Say when we would need to build a B==b' after having rejected a B==b with a wheel due to an incompatibility with A==a, and there's an A==a' with a wheel that may allow us to not try B==b', we should again switch orders and try B==b first, then A==a' (vs. the original A==a then B==b').

We should compile a runnable test suite from the known cases and pick a heuristic according to how well they go.

## Known Cases

I've added them to `scripts/requirements/backtracking`, please add your own.

* Selecting a recent version of numpy backtracks on numba to an ancient version, which picks an ancient llvmlite that fails to build. https://github.com/astral-sh/uv/issues/6281#issuecomment-2316240724, https://github.com/astral-sh/uv/issues/7881, https://github.com/astral-sh/uv/issues/7830
   ```
   numpy>=2.1,<2.2
   numba<=0.60,>0.1
   ```
* Selecting starlette backtracks to a version of fastapi that doesn't depend on starlette at all. https://github.com/astral-sh/uv/issues/1575
   ```
   starlette<=0.36.0
   fastapi<=0.115.2
   ```
* sentry requires `sentry-kafka-schemas>=0.1.50` and `python-rapidjson>=1.4`. We lock `python-rapidjson==1.16` then reject all version of sentry-kafka-schemas because they depend on `python-rapidjson==1.8`, until switching to `python-rapidjson==1.8` and selecting the latest `sentry-kafka-schemas==0.1.113`.
    ```
    python-rapidjson<=1.20,>=1.4
    sentry-kafka-schemas<=0.1.113,>=0.1.50
    ```
* `apache-airflow[all]==2.8.4` (https://github.com/astral-sh/uv/issues/3078) through
    ```
    # Run on Python 3.10
    dill<0.3.9,>=0.2.2
    apache-beam<=2.49.0
    ```

## Example: sentry with sentry-kafka-schemas and python-rapidjson

Trying uncached `uv pip compile` with https://github.com/getsentry/sentry/blob/51281a6abd8ff4a93d2cebc04e1d5fc7aa9c4c11/requirements-base.txt from https://lincolnloop.github.io/python-package-manager-shootout/, the entire right pyramid is incorrect sentry-kafka-schemas fetches (see also https://github.com/pubgrub-rs/pubgrub/issues/263 because we're prefetching too much):

![image](https://github.com/user-attachments/assets/2e33227d-3b5e-4866-ade8-a451677a188f)

Tried 263 versions: sentry-kafka-schemas 62, beautifulsoup4 2, boto3 2, botocore 2, cachetools 2, celery 2, click 2, confluent-kafka 2, croniter 2, cssselect 2, datadog 2, django 2, django-crispy-forms 2, django-csp 2, django-pg-zero-downtime-migrations 2, djangorestframework 2, drf-spectacular 2, email-reply-parser 2, fido2 2, google-api-core 2, google-auth 2, google-cloud-bigtable 2, google-cloud-build 2, google-cloud-core 2, google-cloud-functions 2, google-cloud-kms 2, google-cloud-pubsub 2, google-cloud-spanner 2, google-cloud-storage 2, google-crc32c 2, googleapis-common-protos 2, isodate 2, jsonschema 2, lxml 2, maxminddb 2, mistune 2, mmh3 2, openai 2, packaging 2, parsimonious 2, petname 2, phonenumberslite 2, pillow 2, progressbar2 2, psycopg2-binary 2, pydantic 2, pydantic-core 2, pyjwt 2, pymemcache 2, python-dateutil 2, python-rapidjson 2, python-u2flib-server 2, python3-saml 2, pyuwsgi 2, pyyaml 2, rb 2, redis 2, redis-py-cluster 2, requests 2, requests-oauthlib 2, rfc3339-validator 2, rfc3986-validator 2, sentry-arroyo 2, sentry-ophio 2, sentry-redis-tools 2, sentry-usage-accountant 2, symbolic 2, amqp 1, annotated-types 1, anyio 1, asgiref 1, attrs 1, billiard 1, brotli 1, certifi 1, cffi 1, charset-normalizer 1, click-didyoumean 1, click-plugins 1, click-repl 1, cryptography 1, cssutils 1, distro 1, fastjsonschema 1, google-resumable-media 1, grpc-google-iam-v1 1, grpcio 1, grpcio-status 1, h11 1, hiredis 1, httpcore 1, httpx 1, idna 1, inflection 1, jmespath 1, jsonschema-specifications 1, kombu 1, milksnake 1, msgpack 1, oauthlib 1, phabricator 1, prompt-toolkit 1, proto-plus 1, protobuf 1, pyasn1 1, pyasn1-modules 1, pycparser 1, python-utils 1, referencing 1, regex 1, rpds-py 1, rsa 1, s3transfer 1, sentry-relay 1, sentry-sdk 1, setuptools 1, simplejson 1, six 1, sniffio 1, snuba-sdk 1, soupsieve 1, sqlparse 1, statsd 1, structlog 1, toronado 1, tqdm 1, typing-extensions 1, tzdata 1, ua-parser 1, unidiff 1, uritemplate 1, urllib3 1, vine 1, wcwidth 1, xmlsec 1, zstandard 1


If we inject a `python-rapidjson==1.8` constraint, it goes from 11s to 4.5s; The time is then purely metadata fetching, with a performance penalty for the index not supporting PEP 658:

![image](https://github.com/user-attachments/assets/1b950069-4e42-4376-9153-2cee79141639)

Tried 137 versions: sentry-redis-tools 2, amqp 1, annotated-types 1, anyio 1, asgiref 1, attrs 1, beautifulsoup4 1, billiard 1, boto3 1, botocore 1, brotli 1, cachetools 1, celery 1, certifi 1, cffi 1, charset-normalizer 1, click 1, click-didyoumean 1, click-plugins 1, click-repl 1, confluent-kafka 1, croniter 1, cryptography 1, cssselect 1, cssutils 1, datadog 1, distro 1, django 1, django-crispy-forms 1, django-csp 1, django-pg-zero-downtime-migrations 1, djangorestframework 1, drf-spectacular 1, email-reply-parser 1, fastjsonschema 1, fido2 1, google-api-core 1, google-auth 1, google-cloud-bigtable 1, google-cloud-build 1, google-cloud-core 1, google-cloud-functions 1, google-cloud-kms 1, google-cloud-pubsub 1, google-cloud-spanner 1, google-cloud-storage 1, google-crc32c 1, google-resumable-media 1, googleapis-common-protos 1, grpc-google-iam-v1 1, grpcio 1, grpcio-status 1, h11 1, hiredis 1, httpcore 1, httpx 1, idna 1, inflection 1, isodate 1, jmespath 1, jsonschema 1, jsonschema-specifications 1, kombu 1, lxml 1, maxminddb 1, milksnake 1, mistune 1, mmh3 1, msgpack 1, oauthlib 1, openai 1, packaging 1, parsimonious 1, petname 1, phabricator 1, phonenumberslite 1, pillow 1, progressbar2 1, prompt-toolkit 1, proto-plus 1, protobuf 1, psycopg2-binary 1, pyasn1 1, pyasn1-modules 1, pycparser 1, pydantic 1, pydantic-core 1, pyjwt 1, pymemcache 1, python-dateutil 1, python-rapidjson 1, python-u2flib-server 1, python-utils 1, python3-saml 1, pyuwsgi 1, pyyaml 1, rb 1, redis 1, redis-py-cluster 1, referencing 1, regex 1, requests 1, requests-oauthlib 1, rfc3339-validator 1, rfc3986-validator 1, rpds-py 1, rsa 1, s3transfer 1, sentry-arroyo 1, sentry-kafka-schemas 1, sentry-ophio 1, sentry-relay 1, sentry-sdk 1, sentry-usage-accountant 1, setuptools 1, simplejson 1, six 1, sniffio 1, snuba-sdk 1, soupsieve 1, sqlparse 1, statsd 1, structlog 1, symbolic 1, toronado 1, tqdm 1, typing-extensions 1, tzdata 1, ua-parser 1, unidiff 1, uritemplate 1, urllib3 1, vine 1, wcwidth 1, xmlsec 1, zstandard 1


---

_Label `resolver` added by @konstin on 2024-10-13 13:03_

---

_Comment by @notatallshaw on 2024-10-13 17:07_

https://github.com/astral-sh/uv/issues/3078#issuecomment-2062907457

---

_Comment by @notatallshaw on 2024-11-07 15:13_

I've added a comment on an issue of some recent findings I have on where pip finds a good solution but I think uv is actually doing the right thing: https://github.com/astral-sh/uv/issues/3078#issuecomment-2462455805

My plan is to actually make pip behave more like uv, because in general it will produce a faster answer that is more explainable. That isn't to say pip will start producing exactly the same results as uv, many of the other examples here don't change with pip.

---

_Comment by @notatallshaw on 2024-11-21 15:44_

FYI, I'm also building a list of known problematic resolutions with a slightly different goal: https://github.com/notatallshaw/Pip-Resolution-Scenarios-and-Benchmarks/blob/main/scenarios/problematic.toml

I'm testing the performance of pip's resolver against each sceanrio when changes to the resolver are made. I am testing the performance by counting: how many wheels were visted, how many sdists were visited, how many total requirements were visisted, how many total packages were visisted, how many resolution rounds there were, how many resolution rounds failed to pin a package.

I am including known problematic resolutions from pip, but also other resolver/installers like uv, to try and catch if any changes to the pip resolver slip to known problems with other resolvers.

---

_Comment by @charliermarsh on 2024-11-24 02:18_

A related idea from @henryiii on X: if we see that a package introduces an upper bound on some other package in newer versions, we may want to consider respecting that upper bound in _older_ versions of the same package...

For example, if Pandas 2.1.2 introduced the `numpy<2` bound, we may want to respect that same bound when we look at Pandas 2.1.1, etc. (That would probably _also_ solve the problem raised in this PR, though it's obviously a bit more dicey.)

---

_Comment by @notatallshaw on 2024-11-24 03:01_

This suggestion has come up before, on discussing uv's resolution, or more precisely I think I or someone else suggested uv tries to resolve with and without that constraint and see which one is able to resolve.

This was awhile ago now though, before uv had a universal resolvers or anythjng else, perhaps there is more infrastructure in place to do something like that.

I think it's a good idea, and would probably produce better resolutions in general (undoubtedly there would be edge cases).

---

_Comment by @konstin on 2024-11-26 08:46_

There's two differently shaped problems in this issue:

In some of the cases, an upper bound (or dependency that causes the conflict) is missing on an older version, so we search until we find that old version. This is logically sound, but causes a bad resolution.

In other cases (sentry and airflow), there are upper bounds, but we're getting a lot of conflicts with versions of B until we've exhausted the range and finally start picking a lower version of A.

---

_Comment by @charliermarsh on 2024-11-30 04:28_

I prototyped the "propagate upper bounds" thing, but the problem is that it causes us to backtrack through all versions, so, e.g., I end up failing to build `numba==0.34.0`.

---

_Comment by @notatallshaw on 2024-11-30 04:55_

Give uv resolves so fast, I do wonder if it makes sense on a build failure, rather than exiting resolution instead treat it as a lower bound, there would need to be some explanation to the user though if the resolution was not possible.

---

_Closed by @konstin on 2024-12-16 10:39_

---

_Closed by @konstin on 2024-12-16 10:39_

---
