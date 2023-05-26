# Embedding

Convert unstructured data to embeddings, which will be stored in VectorStore.
Vector similarities can be used to achieve semantic search.

## TextEncoder

The `TextEncoder` converts text inputs to embeddings.
To adapt Langchain agent and vector store, this module must be a Langchain `Embeddings`.

### Configuration

By default, it uses a modified `HuggingFaceEmbeddings` in Langchain.
The modified HuggingFace encoder allows to return normalized embedding(s) by the parameter `norm`.
To configure the default encoder, you change values of parameters via `config.py`.

Modify [embedding configs](embedding/config.py) to configure it, like changing models or disable normalization:

```python
# Text embedding
textencoder_config = {
    'model': 'multi-qa-mpnet-base-cos-v1',
    'norm': True
}
```

### Usage Example

```python
from text_encoder import TextEncoder

encoder = TextEncoder()

# Generate embeddings for a list of documents
doc_embeddings = encoder.embed_documents(['test'])
# Generate embedding for a text input
query_embedding = encoder.embed_query('test')
```

### Customization

You can modify `text_encoder.py` to customize text encoder:

```python
from langchain.embeddings.base import Embeddings


def test_encoder(text: str):
    return [0., 0.]

class TextEncoder(Embeddings):
    def embed_documents(self, texts: List[str]) -> List[List[float]]:
        """Embed search docs."""
        outputs = []
        for t in texts:
            embed = test_encoder(t)
            outputs.append(embed)
        return outputs

    def embed_query(self, text: str) -> List[float]:
        """Embed query text."""
        return test_encoder(text)
```