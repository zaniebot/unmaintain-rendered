```yaml
number: 13381
title: Fix parentheses around return type annotations
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: micha/avoid-parenthesizing-return-type-annotations
created_at: 2024-09-17T13:46:49Z
updated_at: 2024-09-20T07:23:55Z
url: https://github.com/astral-sh/ruff/pull/13381
synced_at: 2026-01-10T21:30:32Z
```

# Fix parentheses around return type annotations

---

_Pull request opened by @MichaReiser on 2024-09-17 13:46_

## Summary

This PR addresses the issue where subscript and other parenthesized expressions in function return type positions were wrapped in an extra pair of parentheses if the function has no parameters. This leads to rather verbose code: 

```patch
 @pytest.fixture
-def mock_legacy_device_tracker_setup() -> (
-    Callable[[HomeAssistant, "MockScanner"], None]
-):
+def mock_legacy_device_tracker_setup() -> Callable[
+    [HomeAssistant, "MockScanner"], None
+]:
     """Return setup callable for legacy device tracker setup."""
     from tests.components.device_tracker.common import mock_legacy_device_tracker_setup

```

It's also inconsistent to how the same return type gets formatted when the function has parameters:

```python
@pytest.fixture
def mock_legacy_device_tracker_setup(
    a,
) -> Callable[
    [HomeAssistant, "MoooooooooooooooooooooooooooooooooooooooooooockScanner"], None
]:
    """Return setup callable for legacy device tracker setup."""
    from tests.components.device_tracker.common import mock_legacy_device_tracker_setup
```

Fixes https://github.com/astral-sh/ruff/issues/9447

## Black deviation

The PR adds two fairly extensive tests that walk through the black deviations. The most notable deviation is that black tries to parenthesize types before splitting the type itself (only applies to subscript)

**Black**
```python
def func() -> (
    list[1, 2222222222222222222, 3333333333333333333333333333333333333333333333333]
):
    pass
```

But it only does so if the function has **no** parameters. Below is Black's formatting if the function has a parameter.

```python

def func(
    a,
) -> list[
    1, 2222222222222222222, 33333333333333333333333333333333333333333333333333333
]:
    pass


def func(
    a,
) -> list[1, 2222222222222222222, 3333333333333333333333333333333333333333333333333333]:
    pass
```

This seems inconsistent. Ruff only parenthesizes the `list` if the subscript value up to the `[` exceeds the line length

**Ruff**

```python
def func() -> list[
    1, 2222222222222222222, 3333333333333333333333333333333333333333333333333
]:
    pass


def func(
    a,
) -> list[
    1, 2222222222222222222, 33333333333333333333333333333333333333333333333333333
]:
    pass


def func(
    a,
) -> list[1, 2222222222222222222, 3333333333333333333333333333333333333333333333333333]:
    pass


# Ruff uses parentheses here similar to Black
def thiiiiiiiiiiiiiiiiiis_iiiiiiiiiiiiiiiiiiiiiiiiiiiiiis_veeeeeeeeeeedooooong() -> (
    list[int, float]
): ...

```

The main counterargument is that most changes in the ecosystem are cases where Black would parenthesize the subscript. I'm interested to get a second opinion on this.

> **Note**: Supporting black's formatting would introduce a fair amount of extra complexity. We couldn't use `maybe_parenthesize_expression` anymore and need to use `best_fitting` to model the three cases (type fits, type parenthesized fits, split the type)


## Preview

This is a preview-only change because it changes the formatting of already formatted code.


## Test Plan

I reformatted some projects locally and skimmed through the changes. Overall, there are only few, and I generally prefer the new formatting (see [this comment](https://github.com/astral-sh/ruff/pull/13381#issuecomment-2355998843)). Whether one prefers Black's formatting with the extra parentheses or not is probably the only "questionable" formatting change. IMO, consistency is more important (it reduces surprises for users: Why is it parenthesized here?).




---

_Label `formatter` added by @MichaReiser on 2024-09-17 13:49_

---

_Label `preview` added by @MichaReiser on 2024-09-17 13:49_

---

_Comment by @github-actions[bot] on 2024-09-17 13:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+44 -58 lines in 7 files in 5 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+3 -3 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_tracker_stores.py#L309'>tests/core/test_tracker_stores.py~L309</a>
```diff
     assert isinstance(tracker_store, InMemoryTrackerStore)
 
 
-async def _tracker_store_and_tracker_with_slot_set() -> (
-    Tuple[InMemoryTrackerStore, DialogueStateTracker]
-):
+async def _tracker_store_and_tracker_with_slot_set() -> Tuple[
+    InMemoryTrackerStore, DialogueStateTracker
+]:
     # returns an InMemoryTrackerStore containing a tracker with a slot set
 
     slot_key = "cuisine"
```

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -3 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/apache/superset/blob/cd8b56706bce667d768456a29cdc85a3fa27ab7c/tests/unit_tests/db_engine_specs/test_ocient.py#L223'>tests/unit_tests/db_engine_specs/test_ocient.py~L223</a>
```diff
     assert result == [expected]
 
 
-def _generate_gis_type_sanitization_test_cases() -> (
-    list[tuple[str, int, Any, dict[str, Any]]]
-):
+def _generate_gis_type_sanitization_test_cases() -> list[
+    tuple[str, int, Any, dict[str, Any]]
+]:
     if not ocient_is_installed():
         return []
 
```

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+8 -10 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/langchain-ai/langchain/blob/a47b33284152d85d2233f069f993f9c0ef4da0d7/libs/community/tests/integration_tests/retrievers/docarray/fixtures.py#L22'>libs/community/tests/integration_tests/retrievers/docarray/fixtures.py~L22</a>
```diff
 
 
 @pytest.fixture
-def init_weaviate() -> (
-    Generator[
-        Tuple[WeaviateDocumentIndex, Dict[str, Any], FakeEmbeddings],
-        None,
-        None,
-    ]
-):
+def init_weaviate() -> Generator[
+    Tuple[WeaviateDocumentIndex, Dict[str, Any], FakeEmbeddings],
+    None,
+    None,
+]:
     """
     cd tests/integration_tests/vectorstores/docker-compose
     docker compose -f weaviate.yml up
```
<a href='https://github.com/langchain-ai/langchain/blob/a47b33284152d85d2233f069f993f9c0ef4da0d7/libs/community/tests/integration_tests/retrievers/docarray/fixtures.py#L73'>libs/community/tests/integration_tests/retrievers/docarray/fixtures.py~L73</a>
```diff
 
 
 @pytest.fixture
-def init_elastic() -> (
-    Generator[Tuple[ElasticDocIndex, Dict[str, Any], FakeEmbeddings], None, None]
-):
+def init_elastic() -> Generator[
+    Tuple[ElasticDocIndex, Dict[str, Any], FakeEmbeddings], None, None
+]:
     """
     cd tests/integration_tests/vectorstores/docker-compose
     docker-compose -f elasticsearch.yml up
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+6 -6 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/3cf1e77ee7e5dc89107d6baab879b6c2832051d2/rotkehlchen/chain/evm/decoding/velodrome/velodrome_cache.py#L149'>rotkehlchen/chain/evm/decoding/velodrome/velodrome_cache.py~L149</a>
```diff
     return pool_addresses, gauge_addresses
 
 
-def read_velodrome_pools_and_gauges_from_cache() -> (
-    tuple[set[ChecksumEvmAddress], set[ChecksumEvmAddress]]
-):  # noqa: E501
+def read_velodrome_pools_and_gauges_from_cache() -> tuple[
+    set[ChecksumEvmAddress], set[ChecksumEvmAddress]
+]:  # noqa: E501
     """
     Reads globaldb cache and returns:
     - A set of all known addresses of velodrome pools.
```
<a href='https://github.com/rotki/rotki/blob/3cf1e77ee7e5dc89107d6baab879b6c2832051d2/rotkehlchen/chain/evm/decoding/velodrome/velodrome_cache.py#L163'>rotkehlchen/chain/evm/decoding/velodrome/velodrome_cache.py~L163</a>
```diff
     )
 
 
-def read_aerodrome_pools_and_gauges_from_cache() -> (
-    tuple[set[ChecksumEvmAddress], set[ChecksumEvmAddress]]
-):  # noqa: E501
+def read_aerodrome_pools_and_gauges_from_cache() -> tuple[
+    set[ChecksumEvmAddress], set[ChecksumEvmAddress]
+]:  # noqa: E501
     """
     Reads globaldb cache and returns:
     - A set of all known addresses of aerodrome pools.
```

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+24 -36 lines across 3 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/python-trio/trio/blob/b834f73cbdd6b0c087ac2d19c5c47f7101a279bd/src/trio/_tests/test_ssl.py#L864'>src/trio/_tests/test_ssl.py~L864</a>
```diff
     client_ctx: SSLContext,
     https_compatible: bool,
 ) -> None:
-    async def stream_maker() -> (
-        tuple[
-            SSLStream[MemoryStapledStream],
-            SSLStream[MemoryStapledStream],
-        ]
-    ):
+    async def stream_maker() -> tuple[
+        SSLStream[MemoryStapledStream],
+        SSLStream[MemoryStapledStream],
+    ]:
         return ssl_memory_stream_pair(
             client_ctx,
             client_kwargs={"https_compatible": https_compatible},
             server_kwargs={"https_compatible": https_compatible},
         )
 
-    async def clogged_stream_maker() -> (
-        tuple[
-            SSLStream[MyStapledStream],
-            SSLStream[MyStapledStream],
-        ]
-    ):
+    async def clogged_stream_maker() -> tuple[
+        SSLStream[MyStapledStream],
+        SSLStream[MyStapledStream],
+    ]:
         client, server = ssl_lockstep_stream_pair(client_ctx)
         # If we don't do handshakes up front, then we run into a problem in
         # the following situation:
```
<a href='https://github.com/python-trio/trio/blob/b834f73cbdd6b0c087ac2d19c5c47f7101a279bd/src/trio/_tests/test_testing.py#L604'>src/trio/_tests/test_testing.py~L604</a>
```diff
 
     await check_one_way_stream(one_way_stream_maker, None)
 
-    async def half_closeable_stream_maker() -> (
-        tuple[
-            StapledStream[MemorySendStream, MemoryReceiveStream],
-            StapledStream[MemorySendStream, MemoryReceiveStream],
-        ]
-    ):
+    async def half_closeable_stream_maker() -> tuple[
+        StapledStream[MemorySendStream, MemoryReceiveStream],
+        StapledStream[MemorySendStream, MemoryReceiveStream],
+    ]:
         return memory_stream_pair()
 
     await check_half_closeable_stream(half_closeable_stream_maker, None)
```
<a href='https://github.com/python-trio/trio/blob/b834f73cbdd6b0c087ac2d19c5c47f7101a279bd/src/trio/_tests/test_testing.py#L621'>src/trio/_tests/test_testing.py~L621</a>
```diff
 
     await check_one_way_stream(one_way_stream_maker, one_way_stream_maker)
 
-    async def two_way_stream_maker() -> (
-        tuple[
-            StapledStream[SendStream, ReceiveStream],
-            StapledStream[SendStream, ReceiveStream],
-        ]
-    ):
+    async def two_way_stream_maker() -> tuple[
+        StapledStream[SendStream, ReceiveStream],
+        StapledStream[SendStream, ReceiveStream],
+    ]:
         return lockstep_stream_pair()
 
     await check_two_way_stream(two_way_stream_maker, two_way_stream_maker)
```
<a href='https://github.com/python-trio/trio/blob/b834f73cbdd6b0c087ac2d19c5c47f7101a279bd/src/trio/testing/_memory_streams.py#L368'>src/trio/testing/_memory_streams.py~L368</a>
```diff
     return stream1, stream2
 
 
-def memory_stream_pair() -> (
-    tuple[
-        StapledStream[MemorySendStream, MemoryReceiveStream],
-        StapledStream[MemorySendStream, MemoryReceiveStream],
-    ]
-):
+def memory_stream_pair() -> tuple[
+    StapledStream[MemorySendStream, MemoryReceiveStream],
+    StapledStream[MemorySendStream, MemoryReceiveStream],
+]:
     """Create a connected, pure-Python, bidirectional stream with infinite
     buffering and flexible configuration options.
 
```
<a href='https://github.com/python-trio/trio/blob/b834f73cbdd6b0c087ac2d19c5c47f7101a279bd/src/trio/testing/_memory_streams.py#L609'>src/trio/testing/_memory_streams.py~L609</a>
```diff
     return _LockstepSendStream(lbq), _LockstepReceiveStream(lbq)
 
 
-def lockstep_stream_pair() -> (
-    tuple[
-        StapledStream[SendStream, ReceiveStream],
-        StapledStream[SendStream, ReceiveStream],
-    ]
-):
+def lockstep_stream_pair() -> tuple[
+    StapledStream[SendStream, ReceiveStream],
+    StapledStream[SendStream, ReceiveStream],
+]:
     """Create a connected, pure-Python, bidirectional stream where data flows
     in lockstep.
 
```

</p>
</details>




---

_Comment by @MichaReiser on 2024-09-17 14:22_

Some more examples:

```patch
 @cache
-def _register_filesystems() -> (
-    Mapping[
-        str,
-        Callable[[str | None, Properties], AbstractFileSystem] | Callable[[str | None], AbstractFileSystem],
-    ]
-):
+def _register_filesystems() -> Mapping[
+    str,
+    Callable[[str | None, Properties], AbstractFileSystem] | Callable[[str | None], AbstractFileSystem],
+]:
     scheme_to_fs = _BUILTIN_SCHEME_TO_FS.copy()
     with Stats.timer("airflow.io.load_filesystems") as timer:


                 event_stream = exhaust_iterator_and_yield_results_with_exception(self)
 
-        def _threadpool_wrap_map_fn() -> (
-            Iterator[Union[Output, AssetMaterialization, AssetObservation, AssetCheckResult]]
-        ):
+        def _threadpool_wrap_map_fn() -> Iterator[
+            Union[Output, AssetMaterialization, AssetObservation, AssetCheckResult]
+        ]:
             with ThreadPoolExecutor(

                 if stt_audio_buffer:
                     # Send audio in the buffer first to speech-to-text, then move on to stt_stream.
                     # This is basically an async itertools.chain.
-                    async def buffer_then_audio_stream() -> (
-                        AsyncGenerator[ProcessedAudioChunk, None]
-                    ):
+                    async def buffer_then_audio_stream() -> AsyncGenerator[
+                        ProcessedAudioChunk, None
+                    ]:
                         # Buffered audio
                         for chunk in stt_audio_buffer:
                             yield chunk
diff --git a/homeassistant/components/openhome/media_player.py b/homeassistant/components/openhome/media_player.py
index 12e5ed992c..2991bb4ef1 100644
--- a/homeassistant/components/openhome/media_player.py
+++ b/homeassistant/components/openhome/media_player.py
@@ -71,11 +71,9 @@ _ReturnFuncType = Callable[
 ]
 
 
-def catch_request_errors() -> (
-    Callable[
-        [_FuncType[_OpenhomeDeviceT, _P, _R]], _ReturnFuncType[_OpenhomeDeviceT, _P, _R]
-    ]
-):
+def catch_request_errors() -> Callable[
+    [_FuncType[_OpenhomeDeviceT, _P, _R]], _ReturnFuncType[_OpenhomeDeviceT, _P, _R]
+]:
     """Catch TimeoutError, aiohttp.ClientError, UpnpError errors."""
 
     def call_wrapper(
diff --git a/homeassistant/components/recorder/statistics.py b/homeassistant/components/recorder/statistics.py
index 41cf4e22b5..19bdccbf83 100644
--- a/homeassistant/components/recorder/statistics.py
+++ b/homeassistant/components/recorder/statistics.py
@@ -935,12 +935,10 @@ def _reduce_statistics(
     return result
 
 
-def reduce_day_ts_factory() -> (
-    tuple[
-        Callable[[float, float], bool],
-        Callable[[float], tuple[float, float]],
-    ]
-):
+def reduce_day_ts_factory() -> tuple[
+    Callable[[float, float], bool],
+    Callable[[float], tuple[float, float]],
+]:
     """Return functions to match same day and day start end."""
     _boundries: tuple[float, float] = (0, 0)
 
@@ -983,12 +981,10 @@ def _reduce_statistics_per_day(
     )
 
 
-def reduce_week_ts_factory() -> (
-    tuple[
-        Callable[[float, float], bool],
-        Callable[[float], tuple[float, float]],
-    ]
-):
+def reduce_week_ts_factory() -> tuple[
+    Callable[[float, float], bool],
+    Callable[[float], tuple[float, float]],
+]:
     """Return functions to match same week and week start end."""
     _boundries: tuple[float, float] = (0, 0)
 
@@ -1041,12 +1037,10 @@ def _find_month_end_time(timestamp: datetime) -> datetime:
     )
 
 
-def reduce_month_ts_factory() -> (
-    tuple[
-        Callable[[float, float], bool],
-        Callable[[float], tuple[float, float]],
-    ]
-):
+def reduce_month_ts_factory() -> tuple[
+    Callable[[float, float], bool],
+    Callable[[float], tuple[float, float]],
+]:
     """Return functions to match same month and month start end."""
     _boundries: tuple[float, float] = (0, 0)
 
diff --git a/homeassistant/exceptions.py b/homeassistant/exceptions.py
index 1eb964d82b..1bedc6bdac 100644
--- a/homeassistant/exceptions.py
+++ b/homeassistant/exceptions.py
@@ -15,9 +15,9 @@ if TYPE_CHECKING:
 _function_cache: dict[str, Callable[[str, str, dict[str, str] | None], str]] = {}
 
 
-def import_async_get_exception_message() -> (
-    Callable[[str, str, dict[str, str] | None], str]
-):
+def import_async_get_exception_message() -> Callable[
+    [str, str, dict[str, str] | None], str
+]:
     """Return a method that can fetch a translated exception message.
 
     Defaults to English, requires translations to already be cached.

diff --git a/tests/components/conftest.py b/tests/components/conftest.py
index bde8cad5ea..ce24f0d614 100644
--- a/tests/components/conftest.py
+++ b/tests/components/conftest.py
@@ -161,9 +161,9 @@ def mock_legacy_device_scanner() -> "MockScanner":
 
 
 @pytest.fixture
-def mock_legacy_device_tracker_setup() -> (
-    Callable[[HomeAssistant, "MockScanner"], None]
-):
+def mock_legacy_device_tracker_setup() -> Callable[
+    [HomeAssistant, "MockScanner"], None
+]:
     """Return setup callable for legacy device tracker setup."""
     from tests.components.device_tracker.common import mock_legacy_device_tracker_setup

```

---

_@MichaReiser reviewed on 2024-09-17 14:38_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/hug.py`:85 on 2024-09-17 14:38_

This is no longer true. We never add parentheses around lists anymore

---

_Comment by @MichaReiser on 2024-09-17 14:49_

Ruff preserves the parentheses around:

```python
def thiiiiiiiiiiiiiiiiiis_iiiiiiiiiiiiiiiiiiiiiiiiiiiiiis_veeeeeeeeeeedooooong() -> (
    list[int, float]
): ...
```

Edit: The reason why ruff adds parentheses above is because the `(` is already past the configured line length limit. Adding parentheses in this case does seem reasonable because `list` would exceed the line length by more characters than adding `(` and then breaking.

The argument against it is that we don't do so when the function has a parameter:

```python

def a():
    def b():
        def c():
            def d():
                def e():
                    def f():
                        def g():
                            def h():
                                def i():
                                    def j():
                                        def k():
                                            def l():
                                                def m():
                                                    def n():
                                                        def o():
                                                            def p():
                                                                def q():
                                                                    def r():
                                                                        def s():
                                                                            def t():
                                                                                def u():
                                                                                    def thiiiiiiiiiiiiiiiiiis_iiiiiiiiiiiiiiiiiiiiiiiiiiiiiis_veeeeeeeeeeedooooong(
                                                                                        a,
                                                                                    ) -> list[
                                                                                        int,
                                                                                        float,
                                                                                    ]: ...
```


But adding a `(` would have a diminishing return in this case because it also indents the list by one extra level. 

Overall. This seems rare enough and I think not parenthesizing is the better outcome but parenthesizing in the first helps stay in the configured line length (as much as possible)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__return_annotation.py.snap`:582 on 2024-09-18 16:09_

This looks surprising but it matches black's formatting

---

_@MichaReiser reviewed on 2024-09-18 16:09_

---

_Marked ready for review by @MichaReiser on 2024-09-18 16:09_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-09-18 16:11_

---

_Comment by @MichaReiser on 2024-09-18 16:31_

Whoops, I must have missed a preview check somewhere.

---

_@charliermarsh approved on 2024-09-19 01:37_

Interesting... I do prefer what you implemented here. I was looking back at https://github.com/astral-sh/ruff/pull/6436 -- I'm wondering why we didn't make this change then? I guess we were biasing towards improving compatibility as much as possible, despite us not understanding why Black had different styling at that point in time.

---

_Comment by @MichaReiser on 2024-09-19 08:22_

> Interesting... I do prefer what you implemented here. I was looking back at #6436 -- I'm wondering why we didn't make this change then? I guess we were biasing towards improving compatibility as much as possible, despite us not understanding why Black had different styling at that point in time.

Looking through the tests added in #6436 the once that are relevant for this change are

```diff
-def xxxxxxxxxxxxxxxxxxxxxxxxxxxx() -> (
-    Set[
-        "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
-    ]
-): ...
+def xxxxxxxxxxxxxxxxxxxxxxxxxxxx() -> Set[
+    "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
+]: ...
 
 
-def xxxxxxxxxxxxxxxxxxxxxxxxxxxx() -> (
-    Set[
-        "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
-    ]
-): ...
+def xxxxxxxxxxxxxxxxxxxxxxxxxxxx() -> Set[
+    "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
+]: ...
 
 
-def xxxxxxxxxxxxxxxxxxxxxxxxxxxx() -> (
-    Set[
-        "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
-    ]
-): ...
+def xxxxxxxxxxxxxxxxxxxxxxxxxxxx() -> Set[
+    "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
+]: ...
 
 
-def xxxxxxxxxxxxxxxxxxxxxxxxxxxx() -> (
-    Set[
-        "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
-    ]
-): ...
+def xxxxxxxxxxxxxxxxxxxxxxxxxxxx() -> Set[
+    "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
+]: ...
```

These are all cases where black keeps the parentheses. But it removes them as soon as the subscript contains another element

```python
def xxxxxxxxxxxxxxxxxxxxxxxxxxxx() -> Set[
    "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
]: ...
 ```

That's why I see this mainly as a refinement of the formatting you implemented in #6436.




---

_Merged by @MichaReiser on 2024-09-20 07:23_

---

_Closed by @MichaReiser on 2024-09-20 07:23_

---

_Branch deleted on 2024-09-20 07:23_

---
