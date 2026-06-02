# embedding

A small Rust HTTP service for generating vector embeddings.

## Quick start

```bash
docker compose up --build
```

First request triggers the model download into `./models` (bind-mounted into the container); subsequent restarts reuse it.

```bash
curl -X POST http://localhost:3000/embed \
  -H 'content-type: application/json' \
  -d '{"texts":["hello world","another piece of text"]}'
```

## Configuration

Configured via environment variables (set them in `.env`):

| Variable | Default | Description |
| --- | --- | --- |
| `EMBEDDING_PORT` | `3000` | Port the service listens on. |
| `EMBEDDING_MODELS` | `nomic` | Comma-separated list of models to load. |
| `EMBEDDING_CACHE_DIR` | _(default cache)_ | Directory for downloaded model files. |
| `EMBEDDING_POOL_SIZE` | _(memory-derived)_ | Number of model instances per pool. |

## API

### `POST /embed`

Request:

```json
{ "texts": ["string", "..."] }
```

Response:

```json
{
  "model": "NomicEmbedTextV15",
  "embeddings": [[0.012, -0.034, ...], ...],
  "tokens": 17
}
```

Errors:

- `400 Bad Request` — `texts` is empty.
- `500 Internal Server Error` — embedding or tokenizer failure (message in `error` field).
