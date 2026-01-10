---
number: 6934
title: "Can't install uWSGI on macOS with UV python "
type: issue
state: closed
author: mbeijen
labels:
  - bug
assignees: []
created_at: 2024-09-02T11:48:01Z
updated_at: 2025-10-11T01:23:01Z
url: https://github.com/astral-sh/uv/issues/6934
synced_at: 2026-01-10T01:24:08Z
---

# Can't install uWSGI on macOS with UV python 

---

_Issue opened by @mbeijen on 2024-09-02 11:48_

I'm using a UV installed python on macOS and I can't install the uWSGI module with it because it's missing the python headers apparently. I'm on macOS Monterey 12.7.6.

pyproject.toml:
```toml
[project]
name="uwsgitest"
version="1.0"
requires-python = ">=3.12"
```

Full error:
```
% uv add uWSGI
Using Python 3.12.5
Creating virtualenv at: .venv
Resolved 2 packages in 62ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: uwsgi==2.0.26
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
running build
running build_py
copying uwsgidecorators.py -> build/lib
installing to build/bdist.macosx-10.9-x86_64/wheel
running install
using profile: buildconf/default.ini
detected include path: ['/usr/local/include', '/Library/Developer/CommandLineTools/usr/lib/clang/14.0.0/include', '/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include', '/Library/Developer/CommandLineTools/usr/include', '/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks']
Patching "bin_name" to properly install_scripts dir
detected CPU cores: 8
configured CFLAGS: -O2 -I. -Wall -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -DUWSGI_HAS_IFADDRS -DUWSGI_ZLIB -mmacosx-version-min=10.5 -DUWSGI_LOCK_USE_OSX_SPINLOCK -DUWSGI_EVENT_USE_KQUEUE -DUWSGI_EVENT_TIMER_USE_KQUEUE -DUWSGI_EVENT_FILEMONITOR_USE_KQUEUE -DUWSGI_UUID -DUWSGI_VERSION="\"2.0.26\"" -DUWSGI_VERSION_BASE="2" -DUWSGI_VERSION_MAJOR="0" -DUWSGI_VERSION_MINOR="26" -DUWSGI_VERSION_REVISION="0" -DUWSGI_VERSION_CUSTOM="\"\"" -DUWSGI_YAML -DUWSGI_SSL -DUWSGI_XML -DUWSGI_XML_EXPAT -DUWSGI_PLUGIN_DIR="\".\"" -DUWSGI_DECLARE_EMBEDDED_PLUGINS="UDEP(python);UDEP(gevent);UDEP(ping);UDEP(cache);UDEP(nagios);UDEP(rrdtool);UDEP(carbon);UDEP(rpc);UDEP(corerouter);UDEP(fastrouter);UDEP(http);UDEP(signal);UDEP(syslog);UDEP(rsyslog);UDEP(logsocket);UDEP(router_uwsgi);UDEP(router_redirect);UDEP(router_basicauth);UDEP(zergpool);UDEP(redislog);UDEP(mongodblog);UDEP(router_rewrite);UDEP(router_http);UDEP(logfile);UDEP(router_cache);UDEP(rawrouter);UDEP(router_static);UDEP(sslrouter);UDEP(spooler);UDEP(cheaper_busyness);UDEP(symcall);UDEP(transformation_tofile);UDEP(transformation_gzip);UDEP(transformation_chunked);UDEP(transformation_offload);UDEP(router_memcached);UDEP(router_redis);UDEP(router_hash);UDEP(router_expires);UDEP(router_metrics);UDEP(transformation_template);UDEP(stats_pusher_socket);" -DUWSGI_LOAD_EMBEDDED_PLUGINS="ULEP(python);ULEP(gevent);ULEP(ping);ULEP(cache);ULEP(nagios);ULEP(rrdtool);ULEP(carbon);ULEP(rpc);ULEP(corerouter);ULEP(fastrouter);ULEP(http);ULEP(signal);ULEP(syslog);ULEP(rsyslog);ULEP(logsocket);ULEP(router_uwsgi);ULEP(router_redirect);ULEP(router_basicauth);ULEP(zergpool);ULEP(redislog);ULEP(mongodblog);ULEP(router_rewrite);ULEP(router_http);ULEP(logfile);ULEP(router_cache);ULEP(rawrouter);ULEP(router_static);ULEP(sslrouter);ULEP(spooler);ULEP(cheaper_busyness);ULEP(symcall);ULEP(transformation_tofile);ULEP(transformation_gzip);ULEP(transformation_chunked);ULEP(transformation_offload);ULEP(router_memcached);ULEP(router_redis);ULEP(router_hash);ULEP(router_expires);ULEP(router_metrics);ULEP(transformation_template);ULEP(stats_pusher_socket);"
*** uWSGI compiling server core ***
[thread 2][clang] core/utils.o
[thread 1][clang] core/protocol.o
[thread 3][clang] core/socket.o
[thread 4][clang] core/logging.o
[thread 5][clang] core/master.o
[thread 6][clang] core/master_utils.o
[thread 7][clang] core/emperor.o
[thread 0][clang] core/notify.o
[thread 0][clang] core/mule.o
[thread 1][clang] core/subscription.o
[thread 5][clang] core/stats.o
[thread 6][clang] core/sendfile.o
[thread 3][clang] core/async.o
[thread 0][clang] core/master_checks.o
[thread 4][clang] core/fifo.o
[thread 7][clang] core/offload.o
[thread 6][clang] core/io.o
[thread 5][clang] core/static.o
[thread 1][clang] core/websockets.o
[thread 3][clang] core/spooler.o
[thread 4][clang] core/snmp.o
[thread 2][clang] core/exceptions.o
[thread 0][clang] core/config.o
[thread 7][clang] core/setup_utils.o
[thread 1][clang] core/clock.o
[thread 5][clang] core/init.o
[thread 4][clang] core/buffer.o
[thread 2][clang] core/reader.o
[thread 7][clang] core/writer.o
[thread 3][clang] core/alarm.o
[thread 6][clang] core/cron.o
[thread 0][clang] core/hooks.o
[thread 1][clang] core/plugins.o
[thread 5][clang] core/lock.o
[thread 4][clang] core/cache.o
[thread 3][clang] core/daemons.o
[thread 6][clang] core/errors.o
[thread 2][clang] core/hash.o
[thread 7][clang] core/master_events.o
[thread 1][clang] core/chunked.o
[thread 0][clang] core/queue.o
[thread 5][clang] core/event.o
[thread 6][clang] core/signal.o
[thread 2][clang] core/strings.o
[thread 3][clang] core/progress.o
[thread 7][clang] core/timebomb.o
[thread 1][clang] core/ini.o
[thread 0][clang] core/fsmon.o
[thread 5][clang] core/mount.o
[thread 7][clang] core/metrics.o
[thread 3][clang] core/plugins_builder.o
[thread 6][clang] core/sharedarea.o
[thread 2][clang] core/rpc.o
[thread 4][clang] core/gateway.o
[thread 1][clang] core/loop.o
[thread 0][clang] core/cookie.o
[thread 5][clang] core/querystring.o
[thread 3][clang] core/rb_timers.o
[thread 2][clang] core/transformations.o
[thread 4][clang] core/uwsgi.o
[thread 6][clang] proto/base.o
[thread 0][clang] proto/uwsgi.o
[thread 1][clang] proto/http.o
[thread 5][clang] proto/fastcgi.o
[thread 7][clang] proto/scgi.o
[thread 3][clang] proto/puwsgi.o
[thread 2][clang] core/zlib.o
[thread 0][clang] core/yaml.o
[thread 6][clang] core/ssl.o
[thread 5][clang] core/legion.o
[thread 3][clang] core/xmlconf.o
[thread 7][clang] core/dot_h.o
[thread 2][clang] core/config_py.o
*** uWSGI compiling embedded plugins ***
[thread 1][clang] plugins/python/python_plugin.o
[thread 7][clang] plugins/python/pyutils.o
[thread 2][clang] plugins/python/pyloader.o
[thread 0][clang] plugins/python/wsgi_handlers.o
[thread 3][clang] plugins/python/wsgi_headers.o
[thread 6][clang] plugins/python/wsgi_subhandler.o
[thread 7][clang] plugins/python/web3_subhandler.o
[thread 5][clang] plugins/python/pump_subhandler.o
[thread 0][clang] plugins/python/gil.o
[thread 3][clang] plugins/python/uwsgi_pymodule.o
[thread 2][clang] plugins/python/profiler.o
[thread 6][clang] plugins/python/symimporter.o
[thread 4][clang] plugins/python/tracebacker.o
[thread 1][clang] plugins/python/raw.o
[thread 7][clang] plugins/gevent/gevent.o
[thread 0][clang] plugins/gevent/hooks.o
[thread 5][clang] plugins/ping/ping_plugin.o
[thread 2][clang] plugins/cache/cache.o
[thread 6][clang] plugins/nagios/nagios.o
[thread 4][clang] plugins/rrdtool/rrdtool.o
[thread 1][clang] plugins/carbon/carbon.o
[thread 5][clang] plugins/rpc/rpc_plugin.o
[thread 2][clang] plugins/corerouter/cr_common.o
[thread 0][clang] plugins/corerouter/cr_map.o
[thread 6][clang] plugins/corerouter/corerouter.o
[thread 7][clang] plugins/fastrouter/fastrouter.o
[thread 4][clang] plugins/http/http.o
[thread 1][clang] plugins/http/keepalive.o
[thread 5][clang] plugins/http/https.o
[thread 2][clang] plugins/http/spdy3.o
[thread 0][clang] plugins/signal/signal_plugin.o
[thread 3][clang] plugins/syslog/syslog_plugin.o
[thread 7][clang] plugins/rsyslog/rsyslog_plugin.o
[thread 1][clang] plugins/logsocket/logsocket_plugin.o
[thread 6][clang] plugins/router_uwsgi/router_uwsgi.o
[thread 5][clang] plugins/router_redirect/router_redirect.o
[thread 0][clang] plugins/router_basicauth/router_basicauth.o
[thread 4][clang] plugins/zergpool/zergpool.o
[thread 3][clang] plugins/redislog/redislog_plugin.o
[thread 1][clang] plugins/mongodblog/mongodblog_plugin.o
[thread 7][clang] plugins/router_rewrite/router_rewrite.o
[thread 6][clang] plugins/router_http/router_http.o
[thread 2][clang] plugins/logfile/logfile.o
[thread 5][clang] plugins/router_cache/router_cache.o
[thread 0][clang] plugins/rawrouter/rawrouter.o
[thread 4][clang] plugins/router_static/router_static.o
[thread 3][clang] plugins/sslrouter/sslrouter.o
[thread 7][clang] plugins/spooler/spooler_plugin.o
[thread 6][clang] plugins/cheaper_busyness/cheaper_busyness.o
[thread 1][clang] plugins/symcall/symcall_plugin.o
[thread 2][clang] plugins/transformation_tofile/tofile.o
[thread 5][clang] plugins/transformation_gzip/gzip.o
[thread 4][clang] plugins/transformation_chunked/chunked.o
[thread 0][clang] plugins/transformation_offload/offload.o
[thread 7][clang] plugins/router_memcached/router_memcached.o
[thread 2][clang] plugins/router_redis/router_redis.o
[thread 1][clang] plugins/router_hash/router_hash.o
[thread 6][clang] plugins/router_expires/expires.o
[thread 5][clang] plugins/router_metrics/plugin.o
[thread 3][clang] plugins/transformation_template/tt.o
[thread 4][clang] plugins/stats_pusher_socket/plugin.o
*** uWSGI linking ***
clang -o build/bdist.macosx-10.9-x86_64/wheel/uWSGI-2.0.26.data/scripts/uwsgi -L/install/lib -Wl,-rpath,/install/lib core/utils.o core/protocol.o core/socket.o core/logging.o core/master.o core/master_utils.o core/emperor.o core/notify.o core/mule.o core/subscription.o core/stats.o core/sendfile.o core/async.o core/master_checks.o core/fifo.o core/offload.o core/io.o core/static.o core/websockets.o core/spooler.o core/snmp.o core/exceptions.o core/config.o core/setup_utils.o core/clock.o core/init.o core/buffer.o core/reader.o core/writer.o core/alarm.o core/cron.o core/hooks.o core/plugins.o core/lock.o core/cache.o core/daemons.o core/errors.o core/hash.o core/master_events.o core/chunked.o core/queue.o core/event.o core/signal.o core/strings.o core/progress.o core/timebomb.o core/ini.o core/fsmon.o core/mount.o core/metrics.o core/plugins_builder.o core/sharedarea.o core/rpc.o core/gateway.o core/loop.o core/cookie.o core/querystring.o core/rb_timers.o core/transformations.o core/uwsgi.o proto/base.o proto/uwsgi.o proto/http.o proto/fastcgi.o proto/scgi.o proto/puwsgi.o core/zlib.o core/yaml.o core/ssl.o core/legion.o core/xmlconf.o core/dot_h.o core/config_py.o plugins/python/python_plugin.o plugins/python/pyutils.o plugins/python/pyloader.o plugins/python/wsgi_handlers.o plugins/python/wsgi_headers.o plugins/python/wsgi_subhandler.o plugins/python/web3_subhandler.o plugins/python/pump_subhandler.o plugins/python/gil.o plugins/python/uwsgi_pymodule.o plugins/python/profiler.o plugins/python/symimporter.o plugins/python/tracebacker.o plugins/python/raw.o plugins/gevent/gevent.o plugins/gevent/hooks.o plugins/ping/ping_plugin.o plugins/cache/cache.o plugins/nagios/nagios.o plugins/rrdtool/rrdtool.o plugins/carbon/carbon.o plugins/rpc/rpc_plugin.o plugins/corerouter/cr_common.o plugins/corerouter/cr_map.o plugins/corerouter/corerouter.o plugins/fastrouter/fastrouter.o plugins/http/http.o plugins/http/keepalive.o plugins/http/https.o plugins/http/spdy3.o plugins/signal/signal_plugin.o plugins/syslog/syslog_plugin.o plugins/rsyslog/rsyslog_plugin.o plugins/logsocket/logsocket_plugin.o plugins/router_uwsgi/router_uwsgi.o plugins/router_redirect/router_redirect.o plugins/router_basicauth/router_basicauth.o plugins/zergpool/zergpool.o plugins/redislog/redislog_plugin.o plugins/mongodblog/mongodblog_plugin.o plugins/router_rewrite/router_rewrite.o plugins/router_http/router_http.o plugins/logfile/logfile.o plugins/router_cache/router_cache.o plugins/rawrouter/rawrouter.o plugins/router_static/router_static.o plugins/sslrouter/sslrouter.o plugins/spooler/spooler_plugin.o plugins/cheaper_busyness/cheaper_busyness.o plugins/symcall/symcall_plugin.o plugins/transformation_tofile/tofile.o plugins/transformation_gzip/gzip.o plugins/transformation_chunked/chunked.o plugins/transformation_offload/offload.o plugins/router_memcached/router_memcached.o plugins/router_redis/router_redis.o plugins/router_hash/router_hash.o plugins/router_expires/expires.o plugins/router_metrics/plugin.o plugins/transformation_template/tt.o plugins/stats_pusher_socket/plugin.o -lpthread -lm -lz -lssl -lcrypto -lexpat -ldl -framework CoreFoundation -lpython3.12
*** error linking uWSGI ***
--- stderr:
/Users/michiel/.cache/uv/builds-v0/.tmpuAFrsX/lib/python3.12/site-packages/setuptools/_distutils/dist.py:260: UserWarning: Unknown distribution option: 'descriptions'
  warnings.warn(msg)
core/ssl.c:268:26: warning: 'PEM_read_bio_DHparams' is deprecated [-Wdeprecated-declarations]
                DH *dh = PEM_read_bio_DHparams(bio, NULL, NULL, NULL);
                         ^
/usr/local/include/openssl/pem.h:473:21: note: 'PEM_read_bio_DHparams' has been explicitly marked deprecated here
DECLARE_PEM_rw_attr(OSSL_DEPRECATEDIN_3_0, DHparams, DH)
                    ^
/usr/local/include/openssl/macros.h:194:49: note: expanded from macro 'OSSL_DEPRECATEDIN_3_0'
#   define OSSL_DEPRECATEDIN_3_0                OSSL_DEPRECATED(3.0)
                                                ^
/usr/local/include/openssl/macros.h:62:52: note: expanded from macro 'OSSL_DEPRECATED'
#     define OSSL_DEPRECATED(since) __attribute__((deprecated))
                                                   ^
core/ssl.c:273:25: warning: 'DH_free' is deprecated [-Wdeprecated-declarations]
                        DH_free(dh);
                        ^
/usr/local/include/openssl/dh.h:211:1: note: 'DH_free' has been explicitly marked deprecated here
OSSL_DEPRECATEDIN_3_0 void DH_free(DH *dh);
^
/usr/local/include/openssl/macros.h:194:49: note: expanded from macro 'OSSL_DEPRECATEDIN_3_0'
#   define OSSL_DEPRECATEDIN_3_0                OSSL_DEPRECATED(3.0)
                                                ^
/usr/local/include/openssl/macros.h:62:52: note: expanded from macro 'OSSL_DEPRECATED'
#     define OSSL_DEPRECATED(since) __attribute__((deprecated))
                                                   ^
core/ssl.c:279:24: warning: 'EC_KEY_new_by_curve_name' is deprecated [-Wdeprecated-declarations]
        EC_KEY *ecdh = EC_KEY_new_by_curve_name(NID_X9_62_prime256v1);
                       ^
/usr/local/include/openssl/ec.h:1017:1: note: 'EC_KEY_new_by_curve_name' has been explicitly marked deprecated here
OSSL_DEPRECATEDIN_3_0 EC_KEY *EC_KEY_new_by_curve_name(int nid);
^
/usr/local/include/openssl/macros.h:194:49: note: expanded from macro 'OSSL_DEPRECATEDIN_3_0'
#   define OSSL_DEPRECATEDIN_3_0                OSSL_DEPRECATED(3.0)
                                                ^
/usr/local/include/openssl/macros.h:62:52: note: expanded from macro 'OSSL_DEPRECATED'
#     define OSSL_DEPRECATED(since) __attribute__((deprecated))
                                                   ^
core/ssl.c:283:17: warning: 'EC_KEY_free' is deprecated [-Wdeprecated-declarations]
                EC_KEY_free(ecdh);
                ^
/usr/local/include/openssl/ec.h:1022:1: note: 'EC_KEY_free' has been explicitly marked deprecated here
OSSL_DEPRECATEDIN_3_0 void EC_KEY_free(EC_KEY *key);
^
/usr/local/include/openssl/macros.h:194:49: note: expanded from macro 'OSSL_DEPRECATEDIN_3_0'
#   define OSSL_DEPRECATEDIN_3_0                OSSL_DEPRECATED(3.0)
                                                ^
/usr/local/include/openssl/macros.h:62:52: note: expanded from macro 'OSSL_DEPRECATED'
#     define OSSL_DEPRECATED(since) __attribute__((deprecated))
                                                   ^
core/ssl.c:563:9: warning: 'SHA1_Init' is deprecated [-Wdeprecated-declarations]
        SHA1_Init(&sha);
        ^
/usr/local/include/openssl/sha.h:49:1: note: 'SHA1_Init' has been explicitly marked deprecated here
OSSL_DEPRECATEDIN_3_0 int SHA1_Init(SHA_CTX *c);
^
/usr/local/include/openssl/macros.h:194:49: note: expanded from macro 'OSSL_DEPRECATEDIN_3_0'
#   define OSSL_DEPRECATEDIN_3_0                OSSL_DEPRECATED(3.0)
                                                ^
/usr/local/include/openssl/macros.h:62:52: note: expanded from macro 'OSSL_DEPRECATED'
#     define OSSL_DEPRECATED(since) __attribute__((deprecated))
                                                   ^
core/ssl.c:564:9: warning: 'SHA1_Update' is deprecated [-Wdeprecated-declarations]
        SHA1_Update(&sha, src, len);
        ^
/usr/local/include/openssl/sha.h:50:1: note: 'SHA1_Update' has been explicitly marked deprecated here
OSSL_DEPRECATEDIN_3_0 int SHA1_Update(SHA_CTX *c, const void *data, size_t len);
^
/usr/local/include/openssl/macros.h:194:49: note: expanded from macro 'OSSL_DEPRECATEDIN_3_0'
#   define OSSL_DEPRECATEDIN_3_0                OSSL_DEPRECATED(3.0)
                                                ^
/usr/local/include/openssl/macros.h:62:52: note: expanded from macro 'OSSL_DEPRECATED'
#     define OSSL_DEPRECATED(since) __attribute__((deprecated))
                                                   ^
core/ssl.c:565:9: warning: 'SHA1_Final' is deprecated [-Wdeprecated-declarations]
        SHA1_Final((unsigned char *)dst, &sha);
        ^
/usr/local/include/openssl/sha.h:51:1: note: 'SHA1_Final' has been explicitly marked deprecated here
OSSL_DEPRECATEDIN_3_0 int SHA1_Final(unsigned char *md, SHA_CTX *c);
^
/usr/local/include/openssl/macros.h:194:49: note: expanded from macro 'OSSL_DEPRECATEDIN_3_0'
#   define OSSL_DEPRECATEDIN_3_0                OSSL_DEPRECATED(3.0)
                                                ^
/usr/local/include/openssl/macros.h:62:52: note: expanded from macro 'OSSL_DEPRECATED'
#     define OSSL_DEPRECATED(since) __attribute__((deprecated))
                                                   ^
core/ssl.c:571:2: warning: 'MD5_Init' is deprecated [-Wdeprecated-declarations]
        MD5_Init(&md5);
        ^
/usr/local/include/openssl/md5.h:49:1: note: 'MD5_Init' has been explicitly marked deprecated here
OSSL_DEPRECATEDIN_3_0 int MD5_Init(MD5_CTX *c);
^
/usr/local/include/openssl/macros.h:194:49: note: expanded from macro 'OSSL_DEPRECATEDIN_3_0'
#   define OSSL_DEPRECATEDIN_3_0                OSSL_DEPRECATED(3.0)
                                                ^
/usr/local/include/openssl/macros.h:62:52: note: expanded from macro 'OSSL_DEPRECATED'
#     define OSSL_DEPRECATED(since) __attribute__((deprecated))
                                                   ^
core/ssl.c:572:2: warning: 'MD5_Update' is deprecated [-Wdeprecated-declarations]
        MD5_Update(&md5, src, len);
        ^
/usr/local/include/openssl/md5.h:50:1: note: 'MD5_Update' has been explicitly marked deprecated here
OSSL_DEPRECATEDIN_3_0 int MD5_Update(MD5_CTX *c, const void *data, size_t len);
^
/usr/local/include/openssl/macros.h:194:49: note: expanded from macro 'OSSL_DEPRECATEDIN_3_0'
#   define OSSL_DEPRECATEDIN_3_0                OSSL_DEPRECATED(3.0)
                                                ^
/usr/local/include/openssl/macros.h:62:52: note: expanded from macro 'OSSL_DEPRECATED'
#     define OSSL_DEPRECATED(since) __attribute__((deprecated))
                                                   ^
core/ssl.c:573:2: warning: 'MD5_Final' is deprecated [-Wdeprecated-declarations]
        MD5_Final((unsigned char *)dst, &md5);
        ^
/usr/local/include/openssl/md5.h:51:1: note: 'MD5_Final' has been explicitly marked deprecated here
OSSL_DEPRECATEDIN_3_0 int MD5_Final(unsigned char *md, MD5_CTX *c);
^
/usr/local/include/openssl/macros.h:194:49: note: expanded from macro 'OSSL_DEPRECATEDIN_3_0'
#   define OSSL_DEPRECATEDIN_3_0                OSSL_DEPRECATED(3.0)
                                                ^
/usr/local/include/openssl/macros.h:62:52: note: expanded from macro 'OSSL_DEPRECATED'
#     define OSSL_DEPRECATED(since) __attribute__((deprecated))
                                                   ^
core/ssl.c:579:9: warning: 'SHA1_Init' is deprecated [-Wdeprecated-declarations]
        SHA1_Init(&sha);
        ^
/usr/local/include/openssl/sha.h:49:1: note: 'SHA1_Init' has been explicitly marked deprecated here
OSSL_DEPRECATEDIN_3_0 int SHA1_Init(SHA_CTX *c);
^
/usr/local/include/openssl/macros.h:194:49: note: expanded from macro 'OSSL_DEPRECATEDIN_3_0'
#   define OSSL_DEPRECATEDIN_3_0                OSSL_DEPRECATED(3.0)
                                                ^
/usr/local/include/openssl/macros.h:62:52: note: expanded from macro 'OSSL_DEPRECATED'
#     define OSSL_DEPRECATED(since) __attribute__((deprecated))
                                                   ^
core/ssl.c:580:9: warning: 'SHA1_Update' is deprecated [-Wdeprecated-declarations]
        SHA1_Update(&sha, s1, len1);
        ^
/usr/local/include/openssl/sha.h:50:1: note: 'SHA1_Update' has been explicitly marked deprecated here
OSSL_DEPRECATEDIN_3_0 int SHA1_Update(SHA_CTX *c, const void *data, size_t len);
^
/usr/local/include/openssl/macros.h:194:49: note: expanded from macro 'OSSL_DEPRECATEDIN_3_0'
#   define OSSL_DEPRECATEDIN_3_0                OSSL_DEPRECATED(3.0)
                                                ^
/usr/local/include/openssl/macros.h:62:52: note: expanded from macro 'OSSL_DEPRECATED'
#     define OSSL_DEPRECATED(since) __attribute__((deprecated))
                                                   ^
core/ssl.c:581:9: warning: 'SHA1_Update' is deprecated [-Wdeprecated-declarations]
        SHA1_Update(&sha, s2, len2);
        ^
/usr/local/include/openssl/sha.h:50:1: note: 'SHA1_Update' has been explicitly marked deprecated here
OSSL_DEPRECATEDIN_3_0 int SHA1_Update(SHA_CTX *c, const void *data, size_t len);
^
/usr/local/include/openssl/macros.h:194:49: note: expanded from macro 'OSSL_DEPRECATEDIN_3_0'
#   define OSSL_DEPRECATEDIN_3_0                OSSL_DEPRECATED(3.0)
                                                ^
/usr/local/include/openssl/macros.h:62:52: note: expanded from macro 'OSSL_DEPRECATED'
#     define OSSL_DEPRECATED(since) __attribute__((deprecated))
                                                   ^
core/ssl.c:582:9: warning: 'SHA1_Final' is deprecated [-Wdeprecated-declarations]
        SHA1_Final((unsigned char *)dst, &sha);
        ^
/usr/local/include/openssl/sha.h:51:1: note: 'SHA1_Final' has been explicitly marked deprecated here
OSSL_DEPRECATEDIN_3_0 int SHA1_Final(unsigned char *md, SHA_CTX *c);
^
/usr/local/include/openssl/macros.h:194:49: note: expanded from macro 'OSSL_DEPRECATEDIN_3_0'
#   define OSSL_DEPRECATEDIN_3_0                OSSL_DEPRECATED(3.0)
                                                ^
/usr/local/include/openssl/macros.h:62:52: note: expanded from macro 'OSSL_DEPRECATED'
#     define OSSL_DEPRECATED(since) __attribute__((deprecated))
                                                   ^
14 warnings generated.
plugins/python/python_plugin.c:138:76: warning: 'Py_NoSiteFlag' is deprecated [-Wdeprecated-declarations]
        {"no-site", no_argument, 0, "do not import site module", uwsgi_opt_true, &Py_NoSiteFlag, 0},
                                                                                  ^
/Users/michiel/.local/share/uv/python/cpython-3.12.5-macos-x86_64-none/include/python3.12/cpython/pydebug.h:14:1: note: 'Py_NoSiteFlag' has been explicitly marked deprecated here
Py_DEPRECATED(3.12) PyAPI_DATA(int) Py_NoSiteFlag;
^
/Users/michiel/.local/share/uv/python/cpython-3.12.5-macos-x86_64-none/include/python3.12/pyport.h:317:54: note: expanded from macro 'Py_DEPRECATED'
#define Py_DEPRECATED(VERSION_UNUSED) __attribute__((__deprecated__))
                                                     ^
plugins/python/python_plugin.c:254:3: warning: 'Py_SetPythonHome' is deprecated [-Wdeprecated-declarations]
                Py_SetPythonHome(wpyhome);
                ^
/Users/michiel/.local/share/uv/python/cpython-3.12.5-macos-x86_64-none/include/python3.12/pylifecycle.h:40:1: note: 'Py_SetPythonHome' has been explicitly marked deprecated here
Py_DEPRECATED(3.11) PyAPI_FUNC(void) Py_SetPythonHome(const wchar_t *);
^
/Users/michiel/.local/share/uv/python/cpython-3.12.5-macos-x86_64-none/include/python3.12/pyport.h:317:54: note: expanded from macro 'Py_DEPRECATED'
#define Py_DEPRECATED(VERSION_UNUSED) __attribute__((__deprecated__))
                                                     ^
plugins/python/python_plugin.c:278:2: warning: 'Py_SetProgramName' is deprecated [-Wdeprecated-declarations]
        Py_SetProgramName(pname);
        ^
/Users/michiel/.local/share/uv/python/cpython-3.12.5-macos-x86_64-none/include/python3.12/pylifecycle.h:37:1: note: 'Py_SetProgramName' has been explicitly marked deprecated here
Py_DEPRECATED(3.11) PyAPI_FUNC(void) Py_SetProgramName(const wchar_t *);
^
/Users/michiel/.local/share/uv/python/cpython-3.12.5-macos-x86_64-none/include/python3.12/pyport.h:317:54: note: expanded from macro 'Py_DEPRECATED'
#define Py_DEPRECATED(VERSION_UNUSED) __attribute__((__deprecated__))
                                                     ^
plugins/python/python_plugin.c:287:2: warning: 'Py_OptimizeFlag' is deprecated [-Wdeprecated-declarations]
        Py_OptimizeFlag = up.optimize;
        ^
/Users/michiel/.local/share/uv/python/cpython-3.12.5-macos-x86_64-none/include/python3.12/cpython/pydebug.h:13:1: note: 'Py_OptimizeFlag' has been explicitly marked deprecated here
Py_DEPRECATED(3.12) PyAPI_DATA(int) Py_OptimizeFlag;
^
/Users/michiel/.local/share/uv/python/cpython-3.12.5-macos-x86_64-none/include/python3.12/pyport.h:317:54: note: expanded from macro 'Py_DEPRECATED'
#define Py_DEPRECATED(VERSION_UNUSED) __attribute__((__deprecated__))
                                                     ^
plugins/python/pyutils.c:391:2: warning: 'PySys_SetArgv' is deprecated [-Wdeprecated-declarations]
        PySys_SetArgv(up.argc, up.py_argv);
        ^
/Users/michiel/.local/share/uv/python/cpython-3.12.5-macos-x86_64-none/include/python3.12/sysmodule.h:13:1: note: 'PySys_SetArgv' has been explicitly marked deprecated here
Py_DEPRECATED(3.11) PyAPI_FUNC(void) PySys_SetArgv(int, wchar_t **);
^
/Users/michiel/.local/share/uv/python/cpython-3.12.5-macos-x86_64-none/include/python3.12/pyport.h:317:54: note: expanded from macro 'Py_DEPRECATED'
#define Py_DEPRECATED(VERSION_UNUSED) __attribute__((__deprecated__))
                                                     ^
1 warning generated.
4 warnings generated.
ld: warning: directory not found for option '-L/install/lib'
ld: library not found for -lpython3.12
clang: error: linker command failed with exit code 1 (use -v to see invocation)
---
```

---

_Comment by @charliermarsh on 2024-09-02 17:20_

Thanks -- I think this is the same as #6488 so gonna merge in there.

---

_Closed by @charliermarsh on 2024-09-02 17:20_

---

_Label `bug` added by @charliermarsh on 2024-09-02 17:20_

---

_Comment by @gr8jam on 2025-01-29 15:49_

I am having the same problem. I am using the latest `uv` version (`0.5.25`). 

Interestingly, I can install `uwsgi` using basic python tools:

```
mkdir test-uwsgi-install
python3.12 -m venv .venv
source .venv/bin/activate
pip install uwsgi
```

But if I try to do it using `uv`, it fails with exact same error as the OP. 




---

_Comment by @charliermarsh on 2025-01-29 15:51_

Have you attempted to reinstall Python with uv? E.g., `uv python install 3.12 --reinstall`.

---

_Comment by @gr8jam on 2025-01-29 16:50_

Thanks @charliermarsh for such a fast answer! 

In the meantime, I found a workaround which I posted [here](https://github.com/astral-sh/uv/issues/6488#issuecomment-2622026195).

But I was curious if your suggestion would also work. Here's what I did (within my project directory, which includes `pyproject.toml` with `uwsgi` as dependency):

```
$ rm -rf .venv
$ uv cache clean uwsgi
$ uv python install 3.12 --reinstall
$ uv sync
Using CPython 3.12.8
Creating virtual environment at: .venv
Resolved 105 packages in 9ms
   Built uwsgi==2.0.28
Prepared 1 package in 5.34s
Installed 86 packages in 519ms
```

It worked! Much better solution compared to what I have done originally. 😃 Thanks again! 

---

_Comment by @lambdaq on 2025-10-11 01:23_

try install `pyuwsgi` instead of `uwsgi`

---
