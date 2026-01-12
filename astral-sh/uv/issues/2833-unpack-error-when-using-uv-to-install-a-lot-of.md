```yaml
number: 2833
title: Unpack error when using uv to install a lot of big packages in Docker in WSL2
type: issue
state: closed
author: potiuk
labels:
  - bug
assignees: []
created_at: 2024-04-05T14:02:22Z
updated_at: 2024-04-19T21:24:12Z
url: https://github.com/astral-sh/uv/issues/2833
synced_at: 2026-01-12T15:58:40Z
```

# Unpack error when using uv to install a lot of big packages in Docker in WSL2

---

_@potiuk_

Not sure if Docker has an influence here, because I am not a WSL2 user, but it seems that `uv's` pretty aggressive networking use has some negative side effects when you install a lot of big packages  in WSL2 .

This has been reported by quite a few Airflow WSL2 contributors who use our `breeze` development environment and got failures when they tried to build our CI image in WSL2, seems that the default 300 seconds timeout from UV was not enough and increasing the UV_HTTP_TIMEOUT to 900 helped to solve the situation.

## A bit of context - why we need to install big number of big packages sometimes

In Airflow we have a common development environment called `breeze` ("It's a breeze to contirbute to Airflow" is the theme there). This development environment is basically a (rather sophisticated) Python sub-project - wrapper around docker-compose and docker that makes it easy to get a common development environment for everyone, that is 1-1 corresponding to the CI environment we use. Due to the number of dependencies (~700 including the transitive ones)  for the complete suite of Airflow + 90 providers we have (we get them all in one monorepo and develop/hack on them together) - the CI image is big, and we use the image as the ultimate common environment to avoid "works for me" syndrome - on top of local venvs people use. To gain all the speed improvesments, default package installer for the CI image is `uv` since almost the first day it has been public, we still have an option to use `pip` but `uv` is default - which is a tremendous improvement in case you need - for whatever reason - rebuild the image locally rather than use remote docker cache (which is the primarey way so far we improved the experience of synchronising the image between contributors).

So in case you have some new dependencies or if our cache is being refreshed, the contributors might sometimes get into situation where the image needs to be rebuilt and what happens essentially is:

* download and unpack latest airflow repo:  

```shell
curl -fsSL "https://github.com/apache/airflow/archive/main.tar.gz" | tar xz -C "${TEMP_AIRFLOW_DIR}" --strip 1
```

* Install airflow in editable mode (which pulls all the ~700 dependencies:

```shell
uv pip install --editable "${TEMP_AIRFLOW_DIR}[devel-ci]"
```

We do it all with UV_NO_CACHE="true" - simply because having UV cache in this case increases the size of the image by a 1GB or so, and we found that docker layer caching in this case works quite a bit better - especially for people who do not have a good network.

## Problem

The problem is that when running this `uv pip install` command in WSL2's docker, the command reliably and consistently fails (regardless of how good connectivity people have) with:

```
107.8 + uv pip install --python /usr/local/bin/python --editable '/tmp/tmp.Pt7KEZ8PVo[devel-ci]'
115.4 Built 1 editable in 9.05s
245.0 error: Failed to download and build: pyspark==3.5.1
245.0   Caused by: Failed to extract archive
245.0   Caused by: failed to unpack `/tmp/.tmpsE5COZ/built-wheels-v2/.tmp8M9bFw/pyspark-3.5.1/deps/jars/hadoop-client-api-3.3.4.jar`
245.0   Caused by: failed to unpack `pyspark-3.5.1/deps/jars/hadoop-client-api-3.3.4.jar` into `/tmp/.tmpsE5COZ/built-wheels-v2/.tmp8M9bFw/pyspark-3.5.1/deps/jars/hadoop-client-api-3.3.4.jar`
245.0   Caused by: request or response body error: error reading a body from connection: unexpected end of file
245.0   Caused by: error reading a body from connection: unexpected end of file
245.0   Caused by: unexpected end of file
```

Usually it happens for spark. 

After some back-forth we found that what reliably works is increasing the UV_HTTP_TIMEOUT. Getting it to 900 seems to reliably solve the problem (we did not try 600, usually when I suspect timeout being the problem I increase it 3x).

For now I implemented a temporary workaround in `breeze` - by detecting WSL2 and increasing the default timeout to 900 in WSL2 (https://github.com/apache/airflow/pull/38742), but I suspect (and looking at the error) this is caused by some of the optimizations done by `uv` - I believe UV does not initially download all the packages, only some parts of it to retrieve missing metadata (or at least that's what the peek into the code of `uv` showed) - and the error suggests that the problem results from using those partially downloaded packages (that's the intelligent guess at least). Not sure if it can be helped, other than increasing the timeout but possibly there is a race or error condition that triggers it and it could be avoided. 


---

_Comment by @notatallshaw on 2024-04-05 21:47_

FWIW, and I have a very fast PC and network so perhaps that does make the difference, I couldn't reproduce this on WSL2 Docker (though docker was slower than not docker).

I tried `breeze --python 3.8 --backend mysql --mysql-version 8.0 --uv-http-timeout 180`

And then I tried this simplified docker image to see each relevant step:

```dockerfile
FROM python:3.11

WORKDIR "/home"

ENV DEBIAN_FRONTEND="noninteractive"
RUN apt-get -y update
RUN apt-get -y install curl apt-transport-https apt-utils build-essential \
ca-certificates dirmngr freetds-bin freetds-dev git graphviz graphviz-dev \
krb5-user ldap-utils libev4 libev-dev libffi-dev libgeos-dev libkrb5-dev \
libldap2-dev libleveldb1d libleveldb-dev libsasl2-2 libsasl2-dev \
libsasl2-modules libssl-dev libxmlsec1 libxmlsec1-dev locales lsb-release \
openssh-client pkgconf sasl2-bin software-properties-common sqlite3 sudo \
unixodbc unixodbc-dev zlib1g-dev

ENV TEMP_AIRFLOW_DIR="/home"
RUN curl -fsSL "https://github.com/apache/airflow/archive/main.tar.gz" | tar xz -C "${TEMP_AIRFLOW_DIR}" --strip 1

ENV CARGO_HOME="/home/.cargo"
RUN curl -LsSf https://astral.sh/uv/install.sh | sh

RUN /home/.cargo/bin/uv venv
RUN /home/.cargo/bin/uv pip install --no-cache --editable "${TEMP_AIRFLOW_DIR}[devel-ci]"
```



---

_Comment by @notatallshaw on 2024-04-05 22:11_

I played the game of how low can I set uv http timeout before airflow devel ci fails with no cache.

For WSL2 I could lower it to ~14 seconds, below that I get:

```
   Built file:///home/damian/airflow_timeout_test                                                                                                  Built 1 editable in 882ms
error: Failed to download and build: pyspark==3.5.1
  Caused by: Failed to extract archive
  Caused by: failed to unpack `/tmp/.tmp0X677T/built-wheels-v2/.tmpDjQ2Nl/pyspark-3.5.1/deps/jars/orc-core-1.9.2-shaded-protobuf.jar`
  Caused by: failed to unpack `pyspark-3.5.1/deps/jars/orc-core-1.9.2-shaded-protobuf.jar` into `/tmp/.tmp0X677T/built-wheels-v2/.tmpDjQ2Nl/pyspark-3.5.1/deps/jars/orc-core-1.9.2-shaded-protobuf.jar`
  Caused by: request or response body error: operation timed out
  Caused by: operation timed out
```

For WSL2 with Docker it's  ~20 seconds, below that I get:

```
 > [8/8] RUN /home/.cargo/bin/uv pip install --no-cache --editable "/home[devel-ci]":                                                                       
1.301 Built 1 editable in 873ms                                                                                                                             
24.71 Resolved 616 packages in 23.40s                                                                                                                       
41.26 error: Failed to download distributions                                                                                                               
41.26   Caused by: Failed to fetch wheel: deltalake==0.16.4                                                                                                 
41.26   Caused by: Failed to extract archive
41.26   Caused by: Failed to download distribution due to network timeout. Try increasing UV_HTTP_TIMEOUT (current value: 15s).
```

So for me it seems to cause ~40% slowdown to use Docker on WSL2, but perhaps on slower (and/or higher latency) connections the network pile up is bigger and the slow down is bigger.

In general this static timeout seems wrong, I'm sure uv maintainers are already aware, and I'm not sure how flexible the HTTP libraries in Rust are, but it would be better if uv could display download progress to the user and to only have a timeout on no progress.

---

_Comment by @charliermarsh on 2024-04-05 22:12_

Yeah, I was chatting with the `reqwest` maintainer, and I think what we're seeing is a difference in how pip and uv measure timeouts (as implemented by `reqwest` vs. Python's `requests`).

In `reqwest`, it's a timeout for the _entire_ request. In `requests`, it's a timeout for each individual read from the socket. The latter seems like what we want.

The `reqwest` maintainer was kind enough to create an issue: https://github.com/seanmonstar/reqwest/issues/2237. So we'll patch that up when it's supported.

---

_Label `bug` added by @charliermarsh on 2024-04-07 00:29_

---

_Comment by @zanieb on 2024-04-19 21:24_

Should be resolved by #3144 let us know if not!

---

_Closed by @zanieb on 2024-04-19 21:24_

---
