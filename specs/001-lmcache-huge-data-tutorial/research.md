# Research: LMCache Huge Data Access Tutorial

## Unknowns & Research Tasks

### 1. vLLM + LMCache Configuration for Prefix Caching
- **Question**: How to explicitly enable and verify prefix caching in vLLM when integrated with LMCache?
- **Finding**: vLLM supports prefix caching via `--enable-prefix-caching`. LMCache enhances this by providing a distributed/persistent KV cache. Integration typically involves setting `LMCACHE_CONFIG_FILE`.
- **Decision**: Use `vllm` as the serving engine with `enable-prefix-caching=True`.

### 2. LangGraph Integration
- **Question**: How to structure the LangGraph node to ensure the static prefix remains at the beginning?
- **Finding**: LangGraph's state management allows passing the context. By defining a "Base Context" node that prepends the manual, we can ensure consistency.
- **Decision**: Use a stateful graph where the first node injects the static "Manual" and subsequent nodes add dynamic instructions.

### 3. LangFuse Observability
- **Question**: Can LangFuse capture KV cache hit rates directly from vLLM/LMCache?
- **Finding**: LangFuse captures standard metrics (latency, tokens). Custom metadata can be added to traces. vLLM returns some info in `usage` or extra fields.
- **Decision**: Manually extract `hit_rate` and `ttft` from the vLLM response (or LMCache logs/API) and push as custom tags/metadata to LangFuse.

### 4. Large Manual Text Handling
- **Question**: What is the optimal token count for the "Manual" to demonstrate significant TTFT reduction?
- **Finding**: Significant differences are usually visible above 5,000-10,000 tokens.
- **Decision**: Use a synthetic or public domain manual of ~15,000 tokens to make the performance difference obvious.

## Best Practices

- **Prefix Alignment**: Always ensure the static text is exactly the same (including whitespace/newlines) across requests to hit the cache.
- **Layered Prompting**: Place system instructions *after* the cached manual if they change frequently, but *before* if they are also static. For this tutorial, we will demonstrate both to show the "cache busting" effect.

## Decision Summary

- **Serving Engine**: vLLM with LMCache.
- **Orchestration**: LangGraph for defining the flow.
- **Observability**: LangFuse for tracing and custom metric visualization.
- **Notebook**: Single `.ipynb` with integrated `matplotlib` charts.
