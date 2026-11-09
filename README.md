# outcrawl-haystack

[![PyPI - Version](https://img.shields.io/pypi/v/outcrawl-haystack.svg)](https://pypi.org/project/outcrawl-haystack)
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/outcrawl-haystack.svg)](https://pypi.org/project/outcrawl-haystack)

[Haystack](https://haystack.deepset.ai/) components for [Outcrawl](https://outcrawl.ai), web infrastructure for AI agents:
search the web and fetch pages as Markdown, as Haystack `Document`s.

- `OutcrawlWebSearch`: runs a web search through the Outcrawl Search API and returns the results as documents, plus their links.
- `OutcrawlFetcher`: loads each URL in a browser through the Outcrawl Scrape API, so JavaScript-rendered pages work, and returns the page as a Markdown document.

## Installation

```console
pip install outcrawl-haystack
```

You need an Outcrawl API key, which you create at [outcrawl.ai/console](https://outcrawl.ai/console). The free plan includes credits every month.
Set it as the `OUTCRAWL_API_KEY` environment variable.

## Usage

```python
from haystack_integrations.components.fetchers.outcrawl import OutcrawlFetcher
from haystack_integrations.components.websearch.outcrawl import OutcrawlWebSearch

search = OutcrawlWebSearch(top_k=5)
found = search.run(query="What is Haystack by deepset?")
print(found["links"])

fetcher = OutcrawlFetcher()
pages = fetcher.run(urls=found["links"][:2])
for doc in pages["documents"]:
    print(doc.meta, doc.content[:200])
```

Both components also have `run_async`, and both accept extra request parameters (`search_params` and `params`);
see the [Outcrawl docs](https://outcrawl.ai/docs) for the options.

## Development

```console
pip install hatch
hatch run fmt-check
hatch run test:unit
```

The integration tests need `OUTCRAWL_API_KEY` in the environment. Releases are published to PyPI by the `Release` workflow when a `v<version>` tag is pushed.

## License

`outcrawl-haystack` is distributed under the terms of the [Apache-2.0](LICENSE) license.
