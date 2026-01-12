```yaml
number: 2118
title: TypedDict discriminated union support
type: issue
state: closed
author: scosman
labels: []
assignees: []
created_at: 2025-12-19T17:34:52Z
updated_at: 2025-12-19T17:40:07Z
url: https://github.com/astral-sh/ty/issues/2118
synced_at: 2026-01-12T15:54:26Z
```

# TypedDict discriminated union support

---

_@scosman_

### Summary

ty has a false positive on TypedDict discriminated unions. Full example below (the error is the last line). 

Expected: since we validate the discriminator, type checking should pass.

Actual: type checking fails, not recognizing the narrowing.

For reference, the same code works in pyright without error.

```
class VectorStoreType(str, Enum):
    LANCE_DB_FTS = "lancedb_fts"
    LANCE_DB_HYBRID = "lancedb_hybrid"
    LANCE_DB_VECTOR = "lancedb_vector"


class LanceDBConfigFTSProperties(TypedDict, total=True):
    store_type: Literal[VectorStoreType.LANCE_DB_FTS]
    similarity_top_k: PositiveInt
    overfetch_factor: PositiveInt
    vector_column_name: str
    text_key: str
    doc_id_key: str


class LanceDBConfigVectorProperties(TypedDict, total=True):
    store_type: Literal[VectorStoreType.LANCE_DB_VECTOR]
    similarity_top_k: PositiveInt
    overfetch_factor: PositiveInt
    vector_column_name: str
    text_key: str
    doc_id_key: str
    nprobes: PositiveInt


class LanceDBConfigHybridProperties(LanceDBConfigVectorProperties, total=True):
    store_type: Literal[VectorStoreType.LANCE_DB_HYBRID]
    # no additional properties for hybrid, it is the same as the vector properties
    pass

class VectorStoreConfig(BaseModel):
    store_type: VectorStoreType = Field(
        description="The type of vector store to use.",
    )
    properties: (
        LanceDBConfigFTSProperties
        | LanceDBConfigVectorProperties
        | LanceDBConfigHybridProperties
    ) = Field(
        description="The properties of the vector store config, specific to the selected store_type.",
        # the discriminator refers to the properties->store_type key (not the store_type field on the parent model)
        discriminator="store_type",
    )

    @property
    def lancedb_vector_properties(self) -> LanceDBConfigVectorProperties:
        if self.properties["store_type"] != VectorStoreType.LANCE_DB_VECTOR:
            raise ValueError(
                f"Lancedb vector properties are only available for LanceDB vector store type. Got {self.properties.get('store_type')}"
            )
        # Type Error Here! `Return type does not match returned value: expected `LanceDBConfigVectorProperties`, found `LanceDBConfigFTSProperties | LanceDBConfigVectorProperties | LanceDBConfigHybridProperties`ty[invalid-return-type](https://ty.dev/rules#invalid-return-type)`
        return self.properties
```

### Version

0.2

---

_Comment by @AlexWaygood on 2025-12-19 17:40_

Thanks! Please see #1479

---

_Closed by @AlexWaygood on 2025-12-19 17:40_

---
