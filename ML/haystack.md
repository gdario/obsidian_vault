## Essential Components

- Nodes.
	- Retriever: fast algorithm to select the most promising candidate documents.
	- Reader: deep network trained for QA.
- Pipelines.
- Agents.
- REST API.

## Getting Started

- There is a utility function `convert_files_to_dicts`.
- Document stores expect documents as dictionaries. See example below.
- Splitting long documents into smaller ones can help speed.

Example of document dictionary

```python
document_store = InMemoryDocumentStore()
dicts = [
	{
		'content': DOCUMENT_TEXT_HERE,
		'meta': {'name': DOCUMENT_NAME, ...}
	}
]
document_store.write_documents(dicts)
```

Simple form of an extractive QA pipeline: `pipe = ExtractiveQAPipeline(reader, retriever)`

## Haystack Concepts

Example scenario: we have an LLM but we want it to go through multiple steps before returning the answer. The agent can be attached to a single node or part of a whole pipeline. Agents seem to be able to answer questions that extractive or generative QA cannot.

### Conversational Agent

It's an agent that keeps track of the queries it receives as well as of the answers it gives. It can manage a set of *tools*. The LLM used for the conversational agent is initialized with a `PromptNode`.

### What tools are we talking about?

A tool can be a Haystack pipeline or a pipeline node. These now include `SearchEngine`, `WebRetriever`, and `TopPSampler`. Users must "describe" the tool to the agent so that it knows when to use them. A tool can be reused multiple times. Each tool must return text the agent can operate on. A return value of `None` breaks the agent.

Agents must be initialized with a `PromptNode`, like `gpt-3.5-turbo` (recommended). The default `PromptTemplate` used by agents is `zero-shot-react`, but it can be modified.