```yaml
number: 14669
title: False positive for PD011
type: issue
state: closed
author: Goldziher
labels: []
assignees: []
created_at: 2024-11-29T06:15:27Z
updated_at: 2024-12-02T01:31:38Z
url: https://github.com/astral-sh/ruff/issues/14669
synced_at: 2026-01-12T15:54:54Z
```

# False positive for PD011

---

_@Goldziher_

I am getting this:

```text
src/utils/embeddings.py:47:17: PD011 Use `.to_numpy()` instead of `.values`
   |
45 |         )
46 |         logger.info("Successfully generated embeddings")
47 |         return [embedding.values for embedding in embeddings]
   |                 ^^^^^^^^^^^^^^^^ PD011
48 |     except ValueError as e:
49 |         logger.error("Failed to get embeddings due to an API error: %s", e)
```

But the type embedding does not have a `.to_numpy` method.

Here is the embedding class:

```python
@dataclasses.dataclass
class TextEmbedding:
    """Text embedding vector and statistics."""

    __module__ = "vertexai.language_models"

    values: List[float]
    statistics: Optional[TextEmbeddingStatistics] = None
    _prediction_response: Optional[aiplatform.models.Prediction] = None

    @classmethod
    def _parse_text_embedding_response(
        cls, prediction_response: aiplatform.models.Prediction, prediction_index: int
    ) -> "TextEmbedding":
        """Creates a `TextEmbedding` object from a prediction.

        Args:
            prediction_response: `aiplatform.models.Prediction` object.

        Returns:
            `TextEmbedding` object.
        """
        prediction = prediction_response.predictions[prediction_index]
        is_prediction_from_pretrained_models = isinstance(
            prediction, collections.abc.Mapping
        )
        if is_prediction_from_pretrained_models:
            embeddings = prediction["embeddings"]
            embedding_stats = embeddings["statistics"]
            return cls(
                values=embeddings["values"],
                statistics=TextEmbeddingStatistics(
                    token_count=embedding_stats["token_count"],
                    truncated=embedding_stats["truncated"],
                ),
                _prediction_response=prediction_response,
            )
        else:
            return cls(values=prediction, _prediction_response=prediction_response)
```

This code is derived from `.venv/lib/python3.12/site-packages/vertexai/language_models/_language_models.py`,





---

_Comment by @MichaReiser on 2024-11-29 07:28_

I think this is the same as https://github.com/astral-sh/ruff/issues/14508



---

_Closed by @charliermarsh on 2024-12-02 01:31_

---
