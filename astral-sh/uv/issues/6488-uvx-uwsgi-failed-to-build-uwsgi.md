---
number: 6488
title: "`uvx uwsgi` failed to build uwsgi"
type: issue
state: closed
author: j178
labels:
  - bug
  - uv python
assignees: []
created_at: 2024-08-23T03:36:02Z
updated_at: 2025-10-11T01:22:52Z
url: https://github.com/astral-sh/uv/issues/6488
synced_at: 2026-01-10T01:24:01Z
---

# `uvx uwsgi` failed to build uwsgi

---

_Issue opened by @j178 on 2024-08-23 03:36_

`uvx uwsgi` fails with this error:
```
--- stderr:
/Users/Jo/.cache/uv/builds-v0/.tmp6Ah5r6/lib/python3.12/site-packages/setuptools/_distutils/dist.py:268: UserWarning: Unknown distribution option: 'descriptions'
  warnings.warn(msg)
ld: warning: search path '/install/lib' not found
ld: library 'python3.12' not found
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

<details>
<summary>
The full error log
</summary>

```
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: uwsgi==2.0.26
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
running build
running build_py
copying uwsgidecorators.py -> build/lib
installing to build/bdist.macosx-11.0-arm64/wheel
running install
using profile: buildconf/default.ini
detected include path: ['/Library/Developer/CommandLineTools/usr/lib/clang/15.0.0/include', '/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include', '/Library/Developer/CommandLineTools/usr/include', '/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks']
Patching "bin_name" to properly install_scripts dir
detected CPU cores: 12
configured CFLAGS: -O2 -I. -Wall -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -DUWSGI_HAS_IFADDRS -DUWSGI_ZLIB -mmacosx-version-min=10.5 -DUWSGI_LOCK_USE_OSX_SPINLOCK -DUWSGI_EVENT_USE_KQUEUE -DUWSGI_EVENT_TIMER_USE_KQUEUE -DUWSGI_EVENT_FILEMONITOR_USE_KQUEUE -I/opt/homebrew/Cellar/pcre2/10.44/include -DUWSGI_PCRE2 -DUWSGI_ROUTING -DUWSGI_UUID -DUWSGI_VERSION="\"2.0.26\"" -DUWSGI_VERSION_BASE="2" -DUWSGI_VERSION_MAJOR="0" -DUWSGI_VERSION_MINOR="26" -DUWSGI_VERSION_REVISION="0" -DUWSGI_VERSION_CUSTOM="\"\"" -DUWSGI_YAML -DUWSGI_XML -DUWSGI_XML_EXPAT -DUWSGI_PLUGIN_DIR="\".\"" -DUWSGI_DECLARE_EMBEDDED_PLUGINS="UDEP(python);UDEP(gevent);UDEP(ping);UDEP(cache);UDEP(nagios);UDEP(rrdtool);UDEP(carbon);UDEP(rpc);UDEP(corerouter);UDEP(fastrouter);UDEP(http);UDEP(signal);UDEP(syslog);UDEP(rsyslog);UDEP(logsocket);UDEP(router_uwsgi);UDEP(router_redirect);UDEP(router_basicauth);UDEP(zergpool);UDEP(redislog);UDEP(mongodblog);UDEP(router_rewrite);UDEP(router_http);UDEP(logfile);UDEP(router_cache);UDEP(rawrouter);UDEP(router_static);UDEP(sslrouter);UDEP(spooler);UDEP(cheaper_busyness);UDEP(symcall);UDEP(transformation_tofile);UDEP(transformation_gzip);UDEP(transformation_chunked);UDEP(transformation_offload);UDEP(router_memcached);UDEP(router_redis);UDEP(router_hash);UDEP(router_expires);UDEP(router_metrics);UDEP(transformation_template);UDEP(stats_pusher_socket);" -DUWSGI_LOAD_EMBEDDED_PLUGINS="ULEP(python);ULEP(gevent);ULEP(ping);ULEP(cache);ULEP(nagios);ULEP(rrdtool);ULEP(carbon);ULEP(rpc);ULEP(corerouter);ULEP(fastrouter);ULEP(http);ULEP(signal);ULEP(syslog);ULEP(rsyslog);ULEP(logsocket);ULEP(router_uwsgi);ULEP(router_redirect);ULEP(router_basicauth);ULEP(zergpool);ULEP(redislog);ULEP(mongodblog);ULEP(router_rewrite);ULEP(router_http);ULEP(logfile);ULEP(router_cache);ULEP(rawrouter);ULEP(router_static);ULEP(sslrouter);ULEP(spooler);ULEP(cheaper_busyness);ULEP(symcall);ULEP(transformation_tofile);ULEP(transformation_gzip);ULEP(transformation_chunked);ULEP(transformation_offload);ULEP(router_memcached);ULEP(router_redis);ULEP(router_hash);ULEP(router_expires);ULEP(router_metrics);ULEP(transformation_template);ULEP(stats_pusher_socket);"
*** uWSGI compiling server core ***
core/utils.o is up to date
core/protocol.o is up to date
core/socket.o is up to date
core/logging.o is up to date
core/master.o is up to date
core/master_utils.o is up to date
core/emperor.o is up to date
core/notify.o is up to date
core/mule.o is up to date
core/subscription.o is up to date
core/stats.o is up to date
core/sendfile.o is up to date
core/async.o is up to date
core/master_checks.o is up to date
core/fifo.o is up to date
core/offload.o is up to date
core/io.o is up to date
core/static.o is up to date
core/websockets.o is up to date
core/spooler.o is up to date
core/snmp.o is up to date
core/exceptions.o is up to date
core/config.o is up to date
core/setup_utils.o is up to date
core/clock.o is up to date
core/init.o is up to date
core/buffer.o is up to date
core/reader.o is up to date
core/writer.o is up to date
core/alarm.o is up to date
core/cron.o is up to date
core/hooks.o is up to date
core/plugins.o is up to date
core/lock.o is up to date
core/cache.o is up to date
core/daemons.o is up to date
core/errors.o is up to date
core/hash.o is up to date
core/master_events.o is up to date
core/chunked.o is up to date
core/queue.o is up to date
core/event.o is up to date
core/signal.o is up to date
core/strings.o is up to date
core/progress.o is up to date
core/timebomb.o is up to date
core/ini.o is up to date
core/fsmon.o is up to date
core/mount.o is up to date
core/metrics.o is up to date
core/plugins_builder.o is up to date
core/sharedarea.o is up to date
core/rpc.o is up to date
core/gateway.o is up to date
core/loop.o is up to date
core/cookie.o is up to date
core/querystring.o is up to date
core/rb_timers.o is up to date
core/transformations.o is up to date
core/uwsgi.o is up to date
proto/base.o is up to date
proto/uwsgi.o is up to date
proto/http.o is up to date
proto/fastcgi.o is up to date
proto/scgi.o is up to date
proto/puwsgi.o is up to date
core/zlib.o is up to date
core/regexp.o is up to date
core/routing.o is up to date
core/yaml.o is up to date
core/xmlconf.o is up to date
[thread 1][clang] core/dot_h.o
[thread 2][clang] core/config_py.o
*** uWSGI compiling embedded plugins ***
plugins/python/python_plugin.o is up to date
plugins/python/pyutils.o is up to date
plugins/python/pyloader.o is up to date
plugins/python/wsgi_handlers.o is up to date
plugins/python/wsgi_headers.o is up to date
plugins/python/wsgi_subhandler.o is up to date
plugins/python/web3_subhandler.o is up to date
plugins/python/pump_subhandler.o is up to date
plugins/python/gil.o is up to date
plugins/python/uwsgi_pymodule.o is up to date
plugins/python/profiler.o is up to date
plugins/python/symimporter.o is up to date
plugins/python/tracebacker.o is up to date
plugins/python/raw.o is up to date
plugins/gevent/gevent.o is up to date
plugins/gevent/hooks.o is up to date
plugins/ping/ping_plugin.o is up to date
plugins/cache/cache.o is up to date
plugins/nagios/nagios.o is up to date
plugins/rrdtool/rrdtool.o is up to date
plugins/carbon/carbon.o is up to date
plugins/rpc/rpc_plugin.o is up to date
plugins/corerouter/cr_common.o is up to date
plugins/corerouter/cr_map.o is up to date
plugins/corerouter/corerouter.o is up to date
plugins/fastrouter/fastrouter.o is up to date
plugins/http/http.o is up to date
plugins/http/keepalive.o is up to date
plugins/http/https.o is up to date
plugins/http/spdy3.o is up to date
plugins/signal/signal_plugin.o is up to date
plugins/syslog/syslog_plugin.o is up to date
plugins/rsyslog/rsyslog_plugin.o is up to date
plugins/logsocket/logsocket_plugin.o is up to date
plugins/router_uwsgi/router_uwsgi.o is up to date
plugins/router_redirect/router_redirect.o is up to date
plugins/router_basicauth/router_basicauth.o is up to date
plugins/zergpool/zergpool.o is up to date
plugins/redislog/redislog_plugin.o is up to date
plugins/mongodblog/mongodblog_plugin.o is up to date
plugins/router_rewrite/router_rewrite.o is up to date
plugins/router_http/router_http.o is up to date
plugins/logfile/logfile.o is up to date
plugins/router_cache/router_cache.o is up to date
plugins/rawrouter/rawrouter.o is up to date
plugins/router_static/router_static.o is up to date
plugins/sslrouter/sslrouter.o is up to date
plugins/spooler/spooler_plugin.o is up to date
plugins/cheaper_busyness/cheaper_busyness.o is up to date
plugins/symcall/symcall_plugin.o is up to date
plugins/transformation_tofile/tofile.o is up to date
plugins/transformation_gzip/gzip.o is up to date
plugins/transformation_chunked/chunked.o is up to date
plugins/transformation_offload/offload.o is up to date
plugins/router_memcached/router_memcached.o is up to date
plugins/router_redis/router_redis.o is up to date
plugins/router_hash/router_hash.o is up to date
plugins/router_expires/expires.o is up to date
plugins/router_metrics/plugin.o is up to date
plugins/transformation_template/tt.o is up to date
plugins/stats_pusher_socket/plugin.o is up to date
*** uWSGI linking ***
clang -o build/bdist.macosx-11.0-arm64/wheel/uWSGI-2.0.26.data/scripts/uwsgi -L/install/lib -Wl,-rpath,/install/lib core/utils.o core/protocol.o core/socket.o core/logging.o core/master.o core/master_utils.o core/emperor.o core/notify.o core/mule.o core/subscription.o core/stats.o core/sendfile.o core/async.o core/master_checks.o core/fifo.o core/offload.o core/io.o core/static.o core/websockets.o core/spooler.o core/snmp.o core/exceptions.o core/config.o core/setup_utils.o core/clock.o core/init.o core/buffer.o core/reader.o core/writer.o core/alarm.o core/cron.o core/hooks.o core/plugins.o core/lock.o core/cache.o core/daemons.o core/errors.o core/hash.o core/master_events.o core/chunked.o core/queue.o core/event.o core/signal.o core/strings.o core/progress.o core/timebomb.o core/ini.o core/fsmon.o core/mount.o core/metrics.o core/plugins_builder.o core/sharedarea.o core/rpc.o core/gateway.o core/loop.o core/cookie.o core/querystring.o core/rb_timers.o core/transformations.o core/uwsgi.o proto/base.o proto/uwsgi.o proto/http.o proto/fastcgi.o proto/scgi.o proto/puwsgi.o core/zlib.o core/regexp.o core/routing.o core/yaml.o core/xmlconf.o core/dot_h.o core/config_py.o plugins/python/python_plugin.o plugins/python/pyutils.o plugins/python/pyloader.o plugins/python/wsgi_handlers.o plugins/python/wsgi_headers.o plugins/python/wsgi_subhandler.o plugins/python/web3_subhandler.o plugins/python/pump_subhandler.o plugins/python/gil.o plugins/python/uwsgi_pymodule.o plugins/python/profiler.o plugins/python/symimporter.o plugins/python/tracebacker.o plugins/python/raw.o plugins/gevent/gevent.o plugins/gevent/hooks.o plugins/ping/ping_plugin.o plugins/cache/cache.o plugins/nagios/nagios.o plugins/rrdtool/rrdtool.o plugins/carbon/carbon.o plugins/rpc/rpc_plugin.o plugins/corerouter/cr_common.o plugins/corerouter/cr_map.o plugins/corerouter/corerouter.o plugins/fastrouter/fastrouter.o plugins/http/http.o plugins/http/keepalive.o plugins/http/https.o plugins/http/spdy3.o plugins/signal/signal_plugin.o plugins/syslog/syslog_plugin.o plugins/rsyslog/rsyslog_plugin.o plugins/logsocket/logsocket_plugin.o plugins/router_uwsgi/router_uwsgi.o plugins/router_redirect/router_redirect.o plugins/router_basicauth/router_basicauth.o plugins/zergpool/zergpool.o plugins/redislog/redislog_plugin.o plugins/mongodblog/mongodblog_plugin.o plugins/router_rewrite/router_rewrite.o plugins/router_http/router_http.o plugins/logfile/logfile.o plugins/router_cache/router_cache.o plugins/rawrouter/rawrouter.o plugins/router_static/router_static.o plugins/sslrouter/sslrouter.o plugins/spooler/spooler_plugin.o plugins/cheaper_busyness/cheaper_busyness.o plugins/symcall/symcall_plugin.o plugins/transformation_tofile/tofile.o plugins/transformation_gzip/gzip.o plugins/transformation_chunked/chunked.o plugins/transformation_offload/offload.o plugins/router_memcached/router_memcached.o plugins/router_redis/router_redis.o plugins/router_hash/router_hash.o plugins/router_expires/expires.o plugins/router_metrics/plugin.o plugins/transformation_template/tt.o plugins/stats_pusher_socket/plugin.o -lpthread -lm -lz -L/opt/homebrew/Cellar/pcre2/10.44/lib -lpcre2-8 -lexpat -ldl -framework CoreFoundation -lpython3.12
*** error linking uWSGI ***
--- stderr:
/Users/Jo/.cache/uv/builds-v0/.tmp6Ah5r6/lib/python3.12/site-packages/setuptools/_distutils/dist.py:268: UserWarning: Unknown distribution option: 'descriptions'
  warnings.warn(msg)
ld: warning: search path '/install/lib' not found
ld: library 'python3.12' not found
clang: error: linker command failed with exit code 1 (use -v to see invocation)
---
```
</details>

This might be a `python-build-standalone` issue?

## Environment

- macOS arm64
- uv 0.3.1


---

_Renamed from "`uvx uwsgi` failed" to "`uvx uwsgi` failed to build uwsgi" by @j178 on 2024-08-23 03:37_

---

_Comment by @charliermarsh on 2024-08-23 03:41_

I can reproduce this, not certain on the cause.

---

_Label `bug` added by @charliermarsh on 2024-08-23 03:41_

---

_Comment by @bluss on 2024-08-23 08:06_

It's the same as good old https://github.com/astral-sh/rye/issues/646

---

_Comment by @zanieb on 2024-08-23 13:19_

Can you use `uvx --python-preference only-system`?

---

_Label `uv python` added by @zanieb on 2024-08-23 13:20_

---

_Comment by @j178 on 2024-08-24 14:21_

> Can you use uvx --python-preference only-system?

Sure. However we currently use the managed Python in our production Docker image because its ease of installation. It would be great if we could use uwsgi with this managed Python setup.

---

_Referenced in [astral-sh/uv#6934](../../astral-sh/uv/issues/6934.md) on 2024-09-02 17:20_

---

_Comment by @wummel on 2024-09-12 06:55_

There is a manual workaround to get uwsgi working by setting environment variables,
at least in my setup using a virtual env created by `uv venv` on a Linux system.
I was able to run `uv pip install uwsgi` with:
```
export CC=gcc
export LDFLAGS=~/.local/share/uv/python/cpython-3.12.6-linux-x86_64-gnu/lib/
```
To be able to use `uv run .venv/bin/uwsgi`, set LD_LIBRARY_PATH:
```
export LD_LIBRARY_PATH=~/.local/share/uv/python/cpython-3.12.6-linux-x86_64-gnu/lib/
```
Replace the part cpython-3.12.6-linux-x86_64-gnu with your uv python version.
Hope this helps some people using C-compiled modules with uv until the underlying issue (wrong python sysconfig data after installation) is fixed.

---

_Comment by @palfrey on 2024-09-29 19:09_

> export LDFLAGS=~/.local/share/uv/python/cpython-3.12.6-linux-x86_64-gnu/lib/

This mostly worked, but needed [`LIBRARY_PATH`](https://stackoverflow.com/a/4250666/320546) not LDFLAGS

---

_Referenced in [astral-sh/uv#8429](../../astral-sh/uv/issues/8429.md) on 2024-10-22 00:39_

---

_Comment by @zanieb on 2024-10-22 01:34_

Tracking in #8429

---

_Closed by @zanieb on 2024-10-22 01:34_

---

_Comment by @gr8jam on 2025-01-29 15:50_

> There is a manual workaround to get uwsgi working by setting environment variables, at least in my setup using a virtual env created by `uv venv` on a Linux system. I was able to run `uv pip install uwsgi` with:
> 
> ```
> export CC=gcc
> export LDFLAGS=~/.local/share/uv/python/cpython-3.12.6-linux-x86_64-gnu/lib/
> ```
> 
> To be able to use `uv run .venv/bin/uwsgi`, set LD_LIBRARY_PATH:
> 
> ```
> export LD_LIBRARY_PATH=~/.local/share/uv/python/cpython-3.12.6-linux-x86_64-gnu/lib/
> ```
> 
> Replace the part cpython-3.12.6-linux-x86_64-gnu with your uv python version. Hope this helps some people using C-compiled modules with uv until the underlying issue (wrong python sysconfig data after installation) is fixed.

~I tried to follow this suggestion, but had no success. Anyone else found a workaround?~

I managed to make it work as well. Here are my steps:
```
$ find /opt/homebrew -name "libpython3.12.dylib"
/opt/homebrew/Cellar/python@3.12/3.12.8/Frameworks/Python.framework/Versions/3.12/lib/libpython3.12.dylib
/opt/homebrew/Cellar/python@3.12/3.12.8/Frameworks/Python.framework/Versions/3.12/lib/python3.12/config-3.12-darwin/libpython3.12.dylib

$ export LIBRARY_PATH=/opt/homebrew/Cellar/python@3.12/3.12.8/Frameworks/Python.framework/Versions/3.12/lib/:$LIBRARY_PATH

$ export CC=gcc

$ uv add uwsgi
```





---

_Referenced in [astral-sh/uv#11864](../../astral-sh/uv/issues/11864.md) on 2025-02-28 19:44_

---

_Referenced in [docker-library/python#1035](../../docker-library/python/issues/1035.md) on 2025-04-29 08:56_

---

_Comment by @lambdaq on 2025-10-11 01:22_

install `pyuwsgi` instead of `uwsgi`

---
